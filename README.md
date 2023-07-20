
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



{
    "pagedResources": {
        "links": [
            {
                "rel": "self",
                "href": "/services/reporting-service/jobs?status=COMPLETED"
            }
        ],
        "content": [
            {
                "jobId": "lBjGVYvvtNBEPbw6O7YsSGGlEuoI5dbDTghW8SPc-lY=",
                "name": "TESTREPORT-Rajesh Chandran",
                "status": "COMPLETED",
                "userId": null,
                "reportType": "UDE",
                "description": null,
                "size": 511,
                "fileType": null,
                "entityName": "Rajya_account Middlename Lastname 50 Char length t",
                "jobEntityId": "34936604",
                "jobEntityGuid": null,
                "jobEntityCd": "A",
                "triggerId": null,
                "submittedBy": "232b4975-d853-4d85-b1e9-431a00e90c2c",
                "createdBy": "CH_A1_C1",
                "legacyJob": false,
                "nearExpiration": false,
                "fileGuid": "0FIL164266b746fe9b2ef9f155619e12d10abe32ce154a2a440827aa3184b2ea432e1685133468",
                "reportFileName": "TESTREPORT-Rajesh_Chandran.tsv",
                "fileFormat": ".tsv",
                "nonCardAccountCd": null,
                "roleOwnerGuid": null,
                "fromDate": "2023-04-26T05:00:00Z",
                "toDate": "2023-05-25T05:00:00Z",
                "nextRunDate": "2023-05-26T20:37:15Z",
                "lastRunDate": "2023-05-26T20:37:48Z",
                "queuedDate": "2023-05-26T20:37:45Z",
                "startDate": "2023-05-26T20:37:47Z",
                "completedDate": "2023-05-26T20:37:48Z",
                "createdOnDate": "2023-05-26T20:37:15Z",
                "lastModifiedDate": "2023-05-26T20:37:49Z",
                "actualScheduleEndDate": null,
                "fromDateFormatted": "4.26.2023 00:00:00 UTC",
                "toDateFormatted": "5.25.2023 00:00:00 UTC",
                "nextRunDateFormatted": "5.27.2023 05:37:15 JST",
                "lastRunDateFormatted": "5.27.2023 05:37:48 JST",
                "queuedDateFormatted": "5.27.2023 05:37:45 JST",
                "startDateFormatted": "5.27.2023 05:37:47 JST",
                "completedDateFormatted": "5.27.2023 05:37:48 JST",
                "createdOnDateFormatted": "5.27.2023 05:37:15 JST",
                "lastModifiedDateFormatted": "5.27.2023 05:37:49 JST",
                "actualScheduleEndDateFormatted": null,
                "rptMigrationEffectiveDate": null,
                "rptMigrationExpirationDate": null,
                "rptMigrationEffective": false,
                "rptMigrationExpired": false,
                "reportAssigned": true,
                "links": [
                    {
                        "rel": "self",
                        "href": "https://stage1.commercial.apps.stl.pcfstage00.mastercard.int/reporting-service/lBjGVYvvtNBEPbw6O7YsSGGlEuoI5dbDTghW8SPc-lY="
                    }
                ],
                "blueprint": {
                    "id": "TlI2UsQGa5djv8eRap96x59YqgEobLW-XlVt1P01W3M=",
                    "name": "TESTREPORT-Rajesh Chandran",
                    "description": null,
                    "blueprintId": "TlI2UsQGa5djv8eRap96x59YqgEobLW-XlVt1P01W3M=",
                    "entities": [
                        {
                            "id": "34936604",
                            "code": "A",
                            "name": null,
                            "entityGuid": null
                        }
                    ],
                    "entityCorpId": "261030",
                    "corpEntityGuid": null,
                    "schemeId": null,
                    "createdByUserId": "232b4975-d853-4d85-b1e9-431a00e90c2c",
                    "updatedByUserId": "232b4975-d853-4d85-b1e9-431a00e90c2c",
                    "frequency": {
                        "frequencyType": "ONCE",
                        "offset": 0,
                        "fromDate": "2023-04-26",
                        "toDate": "2023-05-25",
                        "fromDateFormatted": "4.26.2023",
                        "toDateFormatted": "5.25.2023"
                    },
                    "unifiedFrequency": null,
                    "dateFormat": null,
                    "dateFormatText": null,
                    "numberFormat": null,
                    "numberFormatText": null,
                    "fileFormat": "TAB_DELIMITED",
                    "filterByDateType": "POSTING",
                    "definitionId": "E672401",
                    "definitionProcedureName": null,
                    "definitionEngine": "UDE",
                    "userId": "232b4975-d853-4d85-b1e9-431a00e90c2c",
                    "userLocale": "en_US",
                    "userCobrand": "mastercard",
                    "userAccountDisplayDigits": 4,
                    "userDecimalDisplayDigits": 3,
                    "userRoleId": "4137746",
                    "userRoleEntityId": "34936604",
                    "userRoleEntityType": "A",
                    "emails": [],
                    "deliveryOption": "S",
                    "siteUriText": "https://stage1.sdg2.mastercard.com",
                    "accountStatuses": [],
                    "postedCurrencyCodes": [],
                    "userRoleOwnerGuid": "SROL959C1910B792A54212F75B59E905F88206C6B2DC4AC9917B1B4D9BDD0C89A6D91661984851",
                    "createdDateTime": "2023-05-26T20:37:15Z",
                    "updatedDateTime": "2023-05-26T20:37:16Z",
                    "filters": [],
                    "financialExport": false,
                    "includeSplits": false,
                    "rangePreference": null,
                    "suppressEmailNotification": false,
                    "accountType": "N",
                    "useSingleSupplier": false,
                    "searchBy": null,
                    "searchByValue": null,
                    "reviewStatus": "ALL",
                    "financialsToInclude": null,
                    "groupBy": null,
                    "detailLevel": null,
                    "oboLevel": "myself",
                    "initialRunDateTime": "2023-05-26T20:37:15Z",
                    "lastDownloadDateTime": null,
                    "ftpEnabled": false,
                    "wfFormat": null,
                    "unifiedSchedulingFlag": false,
                    "webfocusScheduleId": null,
                    "expenseRptId": null
                }
            },
            {
                "jobId": "6CX5VRhJBd41EUWwY8I1TNqtqyZ1w_j6e9gbqpePXV0=",
                "name": "Alex Test Report UDE",
                "status": "COMPLETED",
                "userId": null,
                "reportType": "UDE",
                "description": null,
                "size": 4608,
                "fileType": null,
                "entityName": "Rajya_account Middlename Lastname 50 Char length t",
                "jobEntityId": "34936604",
                "jobEntityGuid": null,
                "jobEntityCd": "A",
                "triggerId": null,
                "submittedBy": "232b4975-d853-4d85-b1e9-431a00e90c2c",
                "createdBy": "CH_A1_C1",
                "legacyJob": false,
                "nearExpiration": false,
                "fileGuid": "0FILb7cde2cd3d371fe089dd09a9861a040dc18f8111570ee22ccf7317650df02bd91685131746",
                "reportFileName": "Alex_Test_Report_UDE.xls",
                "fileFormat": ".xls",
                "nonCardAccountCd": null,
                "roleOwnerGuid": null,
                "fromDate": "2023-04-26T05:00:00Z",
                "toDate": "2023-05-25T05:00:00Z",
                "nextRunDate": "2023-05-26T20:08:43Z",
                "lastRunDate": "2023-05-26T20:09:06Z",
                "queuedDate": "2023-05-26T20:09:04Z",
                "startDate": "2023-05-26T20:09:05Z",
                "completedDate": "2023-05-26T20:09:06Z",
                "createdOnDate": "2023-05-26T20:08:43Z",
                "lastModifiedDate": "2023-05-26T20:09:07Z",
                "actualScheduleEndDate": null,
                "fromDateFormatted": "4.26.2023 00:00:00 UTC",
                "toDateFormatted": "5.25.2023 00:00:00 UTC",
                "nextRunDateFormatted": "5.27.2023 05:08:43 JST",
                "lastRunDateFormatted": "5.27.2023 05:09:06 JST",
                "queuedDateFormatted": "5.27.2023 05:09:04 JST",
                "startDateFormatted": "5.27.2023 05:09:05 JST",
                "completedDateFormatted": "5.27.2023 05:09:06 JST",
                "createdOnDateFormatted": "5.27.2023 05:08:43 JST",
                "lastModifiedDateFormatted": "5.27.2023 05:09:07 JST",
                "actualScheduleEndDateFormatted": null,
                "rptMigrationEffectiveDate": null,
                "rptMigrationExpirationDate": null,
                "rptMigrationEffective": false,
                "rptMigrationExpired": false,
                "reportAssigned": true,
                "links": [
                    {
                        "rel": "self",
                        "href": "https://stage1.commercial.apps.stl.pcfstage00.mastercard.int/reporting-service/6CX5VRhJBd41EUWwY8I1TNqtqyZ1w_j6e9gbqpePXV0="
                    }
                ],
                "blueprint": {
                    "id": "zeUMLVrk7iZPYFi_7cTJx7wIfUsoJVgVa4MvLnbIegA=",
                    "name": "Alex Test Report UDE",
                    "description": null,
                    "blueprintId": "zeUMLVrk7iZPYFi_7cTJx7wIfUsoJVgVa4MvLnbIegA=",
                    "entities": [
                        {
                            "id": "34936604",
                            "code": "A",
                            "name": null,
                            "entityGuid": null
                        }
                    ],
                    "entityCorpId": "261030",
                    "corpEntityGuid": null,
                    "schemeId": null,
                    "createdByUserId": "232b4975-d853-4d85-b1e9-431a00e90c2c",
                    "updatedByUserId": "SYSTEM",
                    "frequency": {
                        "frequencyType": "ONCE",
                        "offset": 0,
                        "fromDate": "2023-04-26",
                        "toDate": "2023-05-25",
                        "fromDateFormatted": "4.26.2023",
                        "toDateFormatted": "5.25.2023"
                    },
                    "unifiedFrequency": null,
                    "dateFormat": null,
                    "dateFormatText": null,
                    "numberFormat": null,
                    "numberFormatText": null,
                    "fileFormat": "EXCEL",
                    "filterByDateType": "POSTING",
                    "definitionId": "E58225",
                    "definitionProcedureName": null,
                    "definitionEngine": "UDE",
                    "userId": "232b4975-d853-4d85-b1e9-431a00e90c2c",
                    "userLocale": "en_US",
                    "userCobrand": "mastercard",
                    "userAccountDisplayDigits": 4,
                    "userDecimalDisplayDigits": 3,
                    "userRoleId": "4137746",
                    "userRoleEntityId": "34936604",
                    "userRoleEntityType": "A",
                    "emails": [
                        "LAVANYA.MILTON@MASTERCARD.COM"
                    ],
                    "deliveryOption": "S",
                    "siteUriText": "https://stage1.sdg2.mastercard.com",
                    "accountStatuses": [],
                    "postedCurrencyCodes": [],
                    "userRoleOwnerGuid": "SROL959C1910B792A54212F75B59E905F88206C6B2DC4AC9917B1B4D9BDD0C89A6D91661984851",
                    "createdDateTime": "2023-05-26T20:08:43Z",
                    "updatedDateTime": "2023-05-26T20:31:21Z",
                    "filters": [],
                    "financialExport": false,
                    "includeSplits": false,
                    "rangePreference": null,
                    "suppressEmailNotification": false,
                    "accountType": "N",
                    "useSingleSupplier": false,
                    "searchBy": null,
                    "searchByValue": null,
                    "reviewStatus": "ALL",
                    "financialsToInclude": null,
                    "groupBy": null,
                    "detailLevel": null,
                    "oboLevel": "myself",
                    "initialRunDateTime": "2023-05-26T20:08:43Z",
                    "lastDownloadDateTime": "2023-05-26T20:31:21Z",
                    "ftpEnabled": false,
                    "wfFormat": null,
                    "unifiedSchedulingFlag": false,
                    "webfocusScheduleId": null,
                    "expenseRptId": null
                }
            }
        ],
        "page": {
            "size": 15,
            "totalElements": 2,
            "totalPages": 1,
            "number": 0
        }
    }
}


