here it the news.component.ts: 
import { Component, Input, OnInit } from '@angular/core';
import { QuickLinkCard } from '@shared/components/quick-links-card/quick-link-card.model';
import { Dashboard } from '@features/constants/dashboard-constants';
import { NewsService } from '@features/services/news/news.service';
import { WindowRef } from '@shared/services/windowref/windowref.service';
import { News } from '@features/services/news/news.model';
import { ShimmerService } from '@shared/services/shimmer/shimmer.service';
import { Observable, BehaviorSubject } from 'rxjs';
import { CobrandingService } from '@shared/services/cobranding/cobranding.service';

@Component({
  selector: 'app-news',
  templateUrl: './news.component.html',
  styleUrls: ['./news.component.scss']
})
export class NewsComponent implements OnInit {
  newsDisplayData$ = new BehaviorSubject<QuickLinkCard>(null);
  @Input() isIccpUser = false;
  shimmer = false;
  loadFailure$ = new BehaviorSubject<boolean>(false);
  constructor(
    private readonly newsService: NewsService,
    private readonly winRef: WindowRef,
    private readonly shimmerService: ShimmerService,
    private readonly cobrandingService: CobrandingService
  ) {}
  isLoading$: Observable<boolean>;

  ngOnInit(): void {
    this.fetchNewsData();
  }

  /**
   * Subscribes to the news service and gets the news response
   */
  fetchNewsData() {
    this.isLoading$ = this.shimmerService.isLoading$();
    this.newsService.getNews().subscribe({
      next: (response: News) => {
        const updatedDescription =
          response.listGroupSectionRows && response.listGroupSectionRows.length > 0
            ? 'cardholder.news.viewNews'
            : 'cardholder.news.noNews';
        this.newsDisplayData$.next({
          iconUrl: this.cobrandingService.getCobrandedIconUrl(Dashboard.NEWS_ICON_URL),
          heading: 'cardholder.news.heading',
          description: updatedDescription,
          nextDisplayLink: Dashboard.NEWS_SDNG_PAGE_URL,
          altText: 'cardholder.news.altText',
          customClass: 'news-widget'
        });
        this.loadFailure$.next(true);
      },
      error: error => {
        console.error('Failed to fetch news data from the service:', error);
        this.loadFailure$.next(true);
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

  refresh(): void {
    this.fetchNewsData();
  }
}


Here is quick-links-card.component.ts: 
import { Component, EventEmitter, Input, Output } from '@angular/core';
import { QuickLinkCard } from './quick-link-card.model';
import { COBRAND_PROPERTIES } from '@features/constants/dashboard-constants';

@Component({
  selector: 'app-quick-links-card',
  templateUrl: './quick-links-card.component.html',
  styleUrls: ['./quick-links-card.component.scss']
})
export class QuickLinksCardComponent {
  constructor() {}

  @Input() quickLinkCardDisplay: QuickLinkCard;
  @Input() isIccpUser = false;
  @Input() shimmerLoading: boolean;
  @Input() loadFailure: boolean;
  @Output() emitNextDisplayLink: EventEmitter<string> = new EventEmitter();
  @Output() refresh = new EventEmitter<void>();

  /**
   * When user clicks quick links entire card
   * it should take user to appropriate widget section or any page links
   * @param this.quickLinkCardDisplay.nextDisplayLink that should take user
   * to mentioned page
   */
  navigateToUrl(redirectUrl: string): void {
    this.emitNextDisplayLink.emit(redirectUrl);
  }

  onRefresh(): void {
    this.refresh.emit();
  }

  /**
   *
   * @returns Master card quick link icon background color
   */
  getColor(): string {
    let masterCardBackgroundColor = '';
    if (
      sessionStorage.getItem(COBRAND_PROPERTIES.COBRANDING_SESSION_STORAGE_NAME) ===
      COBRAND_PROPERTIES.COBRAND_FALL_BACK
    ) {
      masterCardBackgroundColor = COBRAND_PROPERTIES.MASTERCARD_QUICKLINK_BACKGROUND;
    }
    return masterCardBackgroundColor;
  }
}

Here is quick-links-card.component.scss:
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
    align-items: center;
    border-radius: 8px;
    display: flex;
    height: 40.5px;
    justify-content: center;
    left: 9px;
    min-width: 40px;
    padding: 0;
    position: relative;
    text-align: unset;
    top: 1px;
    @include media-breakpoint-between(xs, sm) {
      padding: 8px 6px;
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
    @include media-breakpoint-between (md, md) {
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

  &__load-failure-content-container {
    align-items: center;
    align-self: stretch;
    display: flex;
    flex: 1 0 0;
    gap: 12px;

    @include media-breakpoint-between(xs, sm) {
      height: 45px;
      padding: 8px;
      width: 200px;
    }
  }


  &__load-failure-icon-background {
    align-items: center;
    background:#EFEFEE;
    border-radius: 8px;
    display: flex;
    height: 40.5px;
    justify-content: center;
    left: 9px;
    margin-left: 10px;
    position: relative;
    top: 1px;
    width: 40px;

    @include media-breakpoint-between(xs, sm) {
      padding: 8px 6px;
    }
  }


  &__load-failure-icon {
    left: 21.3%;
    margin-bottom: 10px;
    margin-right: 15px;
    position: relative;
    top: 14.3%;
  }

  &__load-failure-content-frame {
    align-items: flex-start;
    display: flex;
    flex: 1 0 0;
    flex-direction: column;
    padding: 12px;

    @include media-breakpoint-between(xs, sm) {
      height: 45px;
      padding: 8px;
      width: 244px;
    }
  }

  &__refresh-button {
    align-items: center;
    border: 1px solid var(--global-base-grays-gray-200, #E3E3E2);
    border-radius: 6px;
    display: flex;
    gap: 8px;
    justify-content: center;
    padding: 8px 24px;

    @include media-breakpoint-between(xs, sm) {
      display: flex;
      padding: 8px 24px;
      justify-content: center;
      align-items: center;
      gap: 8px;
    }
    // @media only screen and (max-width: 320px) {
    //   margin-left: -51px;
    // }
  }

  &__refresh-font {
    color: var(--text-on-light-default, #141413);
    font-family: 'Inter', sans-serif;
    font-size: 14px;
    font-style: normal;
    font-weight: 600;
    line-height: 125%;
    text-align: center;
  }

  &__refresh-icon {
    height: 16px;
    width: 16px;
  }

  &__load-failure-text-in-bold {
    align-self: stretch;
    color: var(--primary-700, #525251);
    display: flex;
    flex-direction: column;
    font-family: 'Inter', sans-serif;
    font-size: 14px;
    font-style: normal;
    font-weight: 700;
    height: 21px;
    justify-content: center;
    line-height: 150%; /* 21px */
    margin: unset;
    overflow: hidden;
    @include media-breakpoint-between(xs, sm) {
      display: flex;
    height: 21px;
    flex-direction: column;
    justify-content: center;
    align-self: stretch;
    }
  }
}

quick-links-card.component.html:
<div
  [ngClass]="{'quickLinksCard__iccp-container': isIccpUser && !shimmerLoading, 'quickLinksCard__shimmer-effect': shimmerLoading }"
  (click)="navigateToUrl(quickLinkCardDisplay?.nextDisplayLink)"
>
<div class="quickLinksCard__text"  *ngIf="!loadFailure">
  <div class="quickLinksCard__iconBackground primary" *ngIf="quickLinkCardDisplay?.iconUrl" 
  [ngStyle]="{'background':getColor()}">
    <img [alt]="quickLinkCardDisplay?.altText | transloco" [src]="quickLinkCardDisplay?.iconUrl" [ngClass]="isIccpUser ? 'quickLinksCard__iccp-icon' : 'quickLinksCard__icon'" />
  </div>
    <div [ngClass]="isIccpUser ? 'quickLinksCard__iccp-text-container' : 'quickLinksCard__text-container'">
     
  
          <p class="quickLinksCard__heading">{{ quickLinkCardDisplay?.heading | transloco }}</p>
          <p [ngClass]="isIccpUser ? 'quickLinksCard__iccp-description' : 'quickLinksCard__description'">
            {{ quickLinkCardDisplay?.description | transloco }}
          </p>
    </div>
</div>
<div class="quickLinksCard__load-failure-content-container" *ngIf="!shimmerLoading">
  <div class="quickLinksCard__load-failure-icon-background" *ngIf="loadFailure" >
  <img class="quickLinksCard__load-failure-icon" src="assets/images/alert-triangle.svg" alt="alert triangles Icon">
    </div> 
<div class="quickLinksCard__load-failure-content-frame" *ngIf="loadFailure" >
  <p class="quickLinksCard__heading">{{ 'cardholder.news.heading' | transloco }}</p>
  <p class="quickLinksCard__load-failure-text-in-bold">{{ 'cardholder.news.errorMessageInBold' | transloco }}
  </p> 
  </div>
  <button class="quickLinksCard__refresh-button" (click)="onRefresh()" *ngIf="loadFailure">
    <img  class="quickLinksCard__refresh-icon" src="assets/images/refresh-icon-black.svg" alt="Refresh Icon">
    <span class="quickLinksCard__refresh-font">{{ 'cardholder.errorPage.button' | transloco}}</span>     
    </button> 
  

   <button *ngIf="!shimmerLoading" type="button" class="quickLinksCard__arrow-button" [attr.aria-label] ="quickLinkCardDisplay?.heading | transloco">
    <svg width="16" height="17" viewBox="0 0 16 17" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path d="M6 12.5L10 8.5L6 4.5" stroke="#141413" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
  </button>
</div>
</div>

news.component.html:
<div class="news__container">
    <app-quick-links-card [quickLinkCardDisplay]="newsDisplayData$ | async"
    [isIccpUser] ="isIccpUser"
    [shimmerLoading]="isLoading$ | async"
    (emitNextDisplayLink) ="navigateToNewsPage($event)"
    [loadFailure]="loadFailure$ | async"
    (refresh) ="fetchNewsData()">
    </app-quick-links-card>
</div>


