import { TestBed, inject } from '@angular/core/testing';
import { HttpClientTestingModule, HttpTestingController } from '@angular/common/http/testing';
import { NewsService } from './news.service';
import { News } from './news.model';
import { ShimmerService } from '@shared/services/shimmer/shimmer.service';
import { Observable, throwError } from 'rxjs';
import { HttpErrorResponse } from '@angular/common/http';
import { catchError } from 'rxjs/operators';

describe('NewsService', () => {
  let newsService: NewsService;
  let httpMock: HttpTestingController;
  let shimmerServiceSpy: jasmine.SpyObj<ShimmerService>;

  beforeEach(() => {
    const shimmerSpy = jasmine.createSpyObj('ShimmerService', ['startLoading', 'stopLoading']);

    TestBed.configureTestingModule({
      imports: [HttpClientTestingModule],
      providers: [
        NewsService,
        { provide: ShimmerService, useValue: shimmerSpy }
      ]
    });

    newsService = TestBed.inject(NewsService);
    httpMock = TestBed.inject(HttpTestingController);
    shimmerServiceSpy = TestBed.inject(ShimmerService) as jasmine.SpyObj<ShimmerService>;
  });

  afterEach(() => {
    httpMock.verify();
  });

  it('should be created', () => {
    expect(newsService).toBeTruthy();
  });

  it('should start and stop shimmer loading while fetching news', async () => {
    const dummyNews: News = { id: 1, title: 'News Title', content: 'News Content' };

    const newsPromise = newsService.getNews().toPromise();

    expect(shimmerServiceSpy.startLoading).toHaveBeenCalled();

    const request = httpMock.expectOne('/sdng/portalservice/retrieveNews.json');
    expect(request.request.method).toBe('GET');
    request.flush(dummyNews);

    const news = await newsPromise;
    expect(news).toEqual(dummyNews);
    expect(shimmerServiceSpy.stopLoading).toHaveBeenCalled();
  });

  it('should handle errors while fetching news', async () => {
    const errorMessage = 'Error fetching news.';
    const errorResponse = new HttpErrorResponse({ 
      status: 500, 
      statusText: 'Internal Server Error', 
      url: '/sdng/portalservice/retrieveNews.json', 
      error: errorMessage 
    });

    const newsPromise = newsService.getNews().pipe(
      catchError((error: any) => {
        expect(error).toEqual(errorResponse);
        expect(shimmerServiceSpy.startLoading).toHaveBeenCalled();
        expect(shimmerServiceSpy.stopLoading).toHaveBeenCalled();
        return throwError(error);
      })
    ).toPromise();

    const request = httpMock.expectOne('/sdng/portalservice/retrieveNews.json');
    expect(request.request.method).toBe('GET');
    request.error(new ErrorEvent('Internal Server Error'), { status: 500, statusText: 'Internal Server Error' });

    try {
      await newsPromise;
      fail('Expected error to be thrown.');
    } catch (error) {
      expect(error).toEqual(errorResponse);
    }
  });
});
