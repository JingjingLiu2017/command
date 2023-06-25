import { ComponentFixture, TestBed } from '@angular/core/testing';
import { NewComponent } from './path-to-your-component'; // update the import path
import { HttpClient } from '@angular/common/http';
import { NewsService } from '@features/services/news/news.service';
import { WindowRef } from '@shared/services/windowref/windowref.service';
import { of, throwError } from 'rxjs';

describe('NewComponent', () => {
  let component: NewComponent;
  let fixture: ComponentFixture<NewComponent>;
  let mockNewsService: jasmine.SpyObj<NewsService>;
  let mockWinRef: jasmine.SpyObj<WindowRef>;

  beforeEach(async () => {
    const newsServiceSpy = jasmine.createSpyObj('NewsService', ['getNews']);
    const windowRefSpy = jasmine.createSpyObj('WindowRef', ['nativeWindow']);

    await TestBed.configureTestingModule({
      declarations: [NewComponent],
      providers: [
        { provide: NewsService, useValue: newsServiceSpy },
        { provide: WindowRef, useValue: windowRefSpy },
        HttpClient,
      ]
    }).compileComponents();

    mockNewsService = TestBed.inject(NewsService) as jasmine.SpyObj<NewsService>;
    mockWinRef = TestBed.inject(WindowRef) as jasmine.SpyObj<WindowRef>;

    fixture = TestBed.createComponent(NewComponent);
    component = fixture.componentInstance;
  });

  it('should create the component', () => {
    expect(component).toBeTruthy();
  });

  it('should fetch news data on initialization', () => {
    mockNewsService.getNews.and.returnValue(of({
      listGroupSectionRows: [1, 2, 3]
    }));
    
    component.ngOnInit();
    expect(component.newsDisplayData.description).toEqual('cardholder.news.viewNews');
  });

  it('should handle no news data', () => {
    mockNewsService.getNews.and.returnValue(of({
      listGroupSectionRows: []
    }));
    
    component.ngOnInit();
    expect(component.newsDisplayData.description).toEqual('cardholder.news.noNews');
  });

  it('should handle error in fetching news data', () => {
    mockNewsService.getNews.and.returnValue(throwError('Error'));
    const consoleSpy = spyOn(console, 'error');
    
    component.ngOnInit();
    expect(consoleSpy).toHaveBeenCalled();
  });

  it('should navigate to news page', () => {
    const fakeUrl = 'http://example.com';
    mockWinRef.nativeWindow = { location: { href: '' } };

    component.navigateToNewsPage(fakeUrl);
    expect(mockWinRef.nativeWindow.location.href).toBe(fakeUrl);
  });
});
