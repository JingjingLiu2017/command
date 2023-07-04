import { TestBed } from '@angular/core/testing';
import { HttpClientTestingModule, HttpTestingController } from '@angular/common/http/testing';
import { NewsService } from './news.service';
import { News } from '../models/news';

describe('NewsService', () => {
  let newsService: NewsService;
  let httpMock: HttpTestingController;

  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [HttpClientTestingModule],
      providers: [NewsService]
    });

    newsService = TestBed.inject(NewsService);
    httpMock = TestBed.inject(HttpTestingController);
  });

  afterEach(() => {
    httpMock.verify();
  });

  it('should be created', () => {
    expect(newsService).toBeTruthy();
  });

  it('should fetch news data', () => {
    const dummyNews: News[] = [
      { id: 1, title: 'News 1', content: 'This is news 1' },
      { id: 2, title: 'News 2', content: 'This is news 2' }
    ];

    newsService.fetchNews().subscribe((news: News[]) => {
      expect(news.length).toBe(2);
      expect(news).toEqual(dummyNews);
    });

    const request = httpMock.expectOne('your-api-url'); // Replace 'your-api-url' with the actual URL for fetching news data
    expect(request.request.method).toBe('GET');
    request.flush(dummyNews);
  });
});
