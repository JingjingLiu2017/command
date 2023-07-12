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

@import '~bootstrap/scss/mixins';
@import '~bootstrap/scss/functions';
@import '~bootstrap/scss/variables';

.quickLinksCard {
  height: 100%;
  overflow: hidden;
  width: 100%;

  &__shimmer-effect {
    background:linear-gradient(
      90deg, rgba(206, 212, 218, .4) 0%, rgba(206, 212, 218, ) 100%
    );
    border-radius: 10px;
    height: 58.67px;
    opacity: .4000000059604645;
  }

  &__container {
    align-items: center;
    background: #F8F8F6;
    border-radius: 8px;
    gap: 12px;
    height: 34.67px;
    padding: 0;

    &:hover {
      background: var(--gray-100, #EFEFEE);
      border-radius: 8px;
    }
  }

  &__iccp-container {
    align-items: center;
    background: #F8F8F6;
    border-radius: 8px;
    display: flex;
    flex-direction: row;
    gap: 12px;
    padding: 0;

    &:hover {
      background: var(--gray-100, #EFEFEE);
      border-radius: 8px;
    }

    @include media-breakpoint-between(xs, sm) {
      height: 58.67px;
    }
    @media only screen and (max-width: 320px) {
      width: 270px;
    }
  }

  &__text {
    align-items: center;
    display: flex;
    flex-direction: row;
    gap: 12px;
    margin-left: 9px;
    width: 100%;
  }

  &__icon {
    left: 21.3%;
    position: relative;
    top: 14.3%;
  }

  &__iconBackground {
    background: #850b08;
    border-radius: 8px;
    height: 40.5px;
    left: 9px;
    position: relative;
    top: 1px;
    width: 40px;

    @include media-breakpoint-between(xs, sm) {
      padding: 8px 6px;
    }
  }

  &__iccp-icon {
    left: 21.3%;
    position: relative;
    top: 23.3%;

    @include media-breakpoint-between(xs, sm) {
      height: 20px;
      left: 10%;
      top: 11%;
      width: 20px;
    }
  }

  &__text-container {
    align-items: flex-start;
    display: flex;
    flex-direction: column;
    padding-left: 12px;
  }

  &__iccp-text-container {
    padding: 12px;

    @include media-breakpoint-between(xs, sm) {
      height: 45px;
      padding: 8px;
      width: 244px;
    }
  }

  &__iccp-heading-description {
    align-items: flex-start;
    display: flex;
    flex-direction: column;
    height: 42px;
    padding: 0;
  }

  &__heading {
    align-items: center;
    color: #141413;
    display: flex;
    font-family: 'Inter', sans-serif;
    font-size: 14px;
    font-style: normal;
    font-weight: 600;
    height: 21px;
    line-height: 150%;

    @include media-breakpoint-between(xs, sm) {
      height: 24px;
      width: 130px;
    }
  }

  &__description {
    align-items: center;
    color: #525251;
    font-family: 'Inter', sans-serif;
    font-size: 14px;
    font-style: normal;
    font-weight: 400;
    height: 21px;
    line-height: 150%;
  }

  &__iccp-description {
    align-items: center;
    color: #525251;
    font-family: 'Inter', sans-serif;
    font-size: 14px;
    font-style: normal;
    font-weight: 400;
    height: 21px;
    line-height: 150%;
    margin: unset;
    @include media-breakpoint-between (xs, sm) {
      display: block;
      height: 21px;
      line-height: 150%;
      margin-top: -4px;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
    }
    @media only screen and (max-width: 320px) {
      width: 167px;
    }
  }

  &__alert-container {
    align-items: center;
    background: #fef3f2;
    border-radius: 50px;
    gap: 8px;
    height: 26px;
    max-width: 77px;
    padding: 4px 8px;
  }

  &__iccp-alert-container {
    background: #fef3f2;
    border-radius: 50px;
    gap: 8px;
    height: 26px;
    padding: 4px 8px;
  }

  &__status-dot {
    height: 6px;
    max-width: 6px;
  }

  &__alert-text {
    color: #141413;
    font-family: 'Inter', sans-serif;
    font-size: 12px;
    font-style: normal;
    font-weight: 500;
    line-height: 125%;
  }

  &__arrow-button {
    align-items: center;
    background: none;
    border: none;
    border-radius: 6px;
    display: flex;
    flex-direction: row;
    gap: 8px;
    height: 32px;
    justify-content: center;
    padding: 8px;

    @include media-breakpoint-between(xs, sm) {
      height: 24px;
      margin-left: -13px;
      padding: 0;
      width: 24px;
    }
    @media only screen and (max-width: 320px) {
      margin-left: -51px;
    }
  }

  &__load-failure-content-container{
    display: flex;
    align-items: center;
    gap: 12px;
    flex: 1 0 0;
    align-self: stretch;
  }
  &__load-failure-icon-background {
    position: relative;
    border-radius: 8px;
    height: 40.5px;
    left: 9px;
    position: relative;
    top: 1px;
    width: 40px;
    background: #656564;

    @include media-breakpoint-between(xs, sm) {
      padding: 8px 6px;
    }
  }

  &____load-failure-icon-container{
    position: relative;
    background-color: transparent;
  }

  &__load-failure-icon{
    width: 24px;
    height: 24px;
    flex-shrink: 0;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 1;

    @include media-breakpoint-between(xs, sm) {
      padding: 8px 6px;
    }
  }

  &__load-failure-content-frame{
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    flex: 1 0 0;
  }

  &__refresh-button{
    display: flex;
    padding: 8px 24px;
    justify-content: center;
    align-items: center;
    gap: 8px;
    border-radius: 6px;
    border: 1px solid var(--global-base-grays-gray-200, #E3E3E2);
  }

  &__refresh-font{
    color: var(--text-on-light-default, #141413);
    text-align: center;
    font-family: Inter;
    font-size: 14px;
    font-style: normal;
    font-weight: 600;
    line-height: 125%;
  }

  &__refresh-icon{
    width: 16px;
    height: 16px;
  }
}

