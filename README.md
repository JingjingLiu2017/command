import { TestBed, inject } from '@angular/core/testing';
import { HttpClientTestingModule, HttpTestingController } from '@angular/common/http/testing';
import { NewsService } from './news.service';
import { News } from './news.model';
import { ShimmerService } from '@shared/services/shimmer/shimmer.service';
import { Observable, of } from 'rxjs';

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

    shimmerServiceSpy.startLoading.and.callThrough();
    shimmerServiceSpy.stopLoading.and.callThrough();

    const newsPromise = newsService.getNews().toPromise();

    expect(shimmerServiceSpy.startLoading).toHaveBeenCalled();

    const request = httpMock.expectOne('/sdng/portalservice/retrieveNews.json');
    expect(request.request.method).toBe('GET');
    request.flush(dummyNews);

    const news = await newsPromise;
    expect(news).toEqual(dummyNews);
    expect(shimmerServiceSpy.stopLoading).toHaveBeenCalled();
  });
});
