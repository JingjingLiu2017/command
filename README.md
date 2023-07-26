import { Component, Input, OnInit } from '@angular/core';
import { Reports } from '@features/services/reports/reports.model';
import { ReportsService } from '@features/services/reports/reports.service';
import { QuickLinkCard } from '@shared/components/quick-links-card/quick-link-card.model';
import { CobrandingService } from '@shared/services/cobranding/cobranding.service';
import { BehaviorSubject, Observable } from 'rxjs';

@Component({
  selector: 'app-reports',
  templateUrl: './reports.component.html',
  styleUrls: ['./reports.component.scss']
})
export class ReportsComponent implements OnInit {
  reportsDisplayData$ = new BehaviorSubject<QuickLinkCard>(null);
  @Input() isIccpUser = false;
  loadFailure$ = new BehaviorSubject<boolean>(false);
  constructor(
    private readonly reportsService: ReportsService,
    private readonly cobrandingService: CobrandingService
  ) {}
  isLoading$: Observable<boolean>;

  ngOnInit(): void {
    this.fetchReportsData();
  }

  /**
   * Subscribes to the Reports service and gets the news response
   */
  fetchReportsData(reportType?: string) {
    this.reportsService.getReports(reportType).subscribe({
      next: (response: Reports) => {
        //need add code for reports widget cards
        this.loadFailure$.next(false);
      },
      error: error => {
        console.error('Failed to fetch reports data from the service:', error);
        this.loadFailure$.next(true);
      }
    });
  }

  refresh(): void {
    this.fetchReportsData();
  }
}
