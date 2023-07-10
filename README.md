here it the news.component.ts: 
import { Component, Input, OnInit } from '@angular/core';
import { QuickLinkCard } from '@shared/components/quick-links-card/quick-link-card.model';
import { Dashboard } from '@features/constants/dashboard-constants';
import { NewsService } from '@features/services/news/news.service';
import { WindowRef } from '@shared/services/windowref/windowref.service';
import { News } from '@features/services/news/news.model';
import { ShimmerService } from '@shared/services/shimmer/shimmer.service';
import { Observable, BehaviorSubject } from 'rxjs';

@Component({
  selector: 'app-news',
  templateUrl: './news.component.html',
  styleUrls: ['./news.component.scss']
})
export class NewsComponent implements OnInit {
  newsDisplayData$ = new BehaviorSubject<QuickLinkCard>(null);
  @Input() isIccpUser = false;
  shimmer = false;
  loadFailure = true;//to test

  constructor(
    private readonly newsService: NewsService,
    private readonly winRef: WindowRef,
    private readonly shimmerService: ShimmerService
  ) {}
  isLoading$: Observable<boolean>;

  ngOnInit(): void {
    this.fetchNewsData();
    this.isLoading$ = this.shimmerService.isLoading$();
  }

  /**
   * Subscribes to the news service and gets the news response
   */
  fetchNewsData() {
    this.newsService.getNews().subscribe({
      next: (response: News) => {
        const updatedDescription =
          response.listGroupSectionRows && response.listGroupSectionRows.length > 0
            ? 'cardholder.news.viewNews'
            : 'cardholder.news.noNews';
        this.newsDisplayData$.next({
          iconUrl: Dashboard.NEWS_ICON_URL,
          heading: 'cardholder.news.heading',
          description: updatedDescription,
          nextDisplayLink: Dashboard.NEWS_SDNG_PAGE_URL,
          altText: 'cardholder.news.altText',
          customClass: 'news-widget'
        });
      },
      error: error => {
        console.error('Failed to fetch news data from the service:', error);
        this.loadFailure = true;
      }
    });
  }

  /**
   * Redirects to the news legacy page when
   * arrow button is clicked from news quick link
   * @param redirectUrl
   */
  navigateToNewsPage(redirectUrl: string): void {
    this.winRef.nativeWindow.location.href = redirectUrl;
  }
}

 
