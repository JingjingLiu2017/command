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

 <div
  [ngClass]="{'quickLinksCard__iccp-container': isIccpUser && !shimmerLoading, 'quickLinksCard__shimmer-effect': shimmerLoading }"
  (click)="navigateToUrl(quickLinkCardDisplay?.nextDisplayLink)"
>
<div class="quickLinksCard__text" *ngIf="!loadFailure">
  <div class="quickLinksCard__iconBackground" *ngIf="quickLinkCardDisplay?.iconUrl">
    <img [alt]="quickLinkCardDisplay?.altText | transloco" [src]="quickLinkCardDisplay?.iconUrl" [ngClass]="isIccpUser ? 'quickLinksCard__iccp-icon' : 'quickLinksCard__icon'" />
  </div>
    <div [ngClass]="isIccpUser ? 'quickLinksCard__iccp-text-container' : 'quickLinksCard__text-container'">
     
  
          <p class="quickLinksCard__heading">{{ quickLinkCardDisplay?.heading | transloco }}</p>
          <p [ngClass]="isIccpUser ? 'quickLinksCard__iccp-description' : 'quickLinksCard__description'">
            {{ quickLinkCardDisplay?.description | transloco }}
          </p>
    </div>
</div>

<div class="quickLinksCard__load-failure-icon-background" *ngIf="loadFailure">
  <img class="quickLinksCard__load-failure-icon" src="assets/images/alert-triangles.svg" alt="alert triangles Icon">
  <p class="quickLinksCard__heading">{{ quickLinkCardDisplay?.heading | transloco }}</p>
  <p class="quickLinksCard__iccp-description">{{ quickLinkCardDisplay?.heading | transloco }}</p> 
<!-- need change -->
  <button class="quickLinksCard__refresh-button" (click)="refresh()">
    <img class="quickLinksCard__refresh-button-icon" src="assets/images/refresh-icon.svg" alt="Refresh Icon">
    <span class="error-page_button-body">{{ 'cardholder.errorPage.button' | transloco}}</span>
</button> 
</div>  

   <button *ngIf="!shimmerLoading" type="button" class="quickLinksCard__arrow-button" [attr.aria-label] ="quickLinkCardDisplay?.heading | transloco">
    <svg width="16" height="17" viewBox="0 0 16 17" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path d="M6 12.5L10 8.5L6 4.5" stroke="#141413" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
  </button>
</div>

