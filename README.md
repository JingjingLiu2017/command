import { ComponentFixture, TestBed } from '@angular/core/testing';
import { BehaviorSubject, of } from 'rxjs';
import { ReportsComponent } from './reports.component';
import { ReportsService } from '@features/services/reports/reports.service';
import { CobrandingService } from '@shared/services/cobranding/cobranding.service';
import { QuickLinkCard } from '@shared/components/quick-links-card/quick-link-card.model';

describe('ReportsComponent', () => {
  let component: ReportsComponent;
  let fixture: ComponentFixture<ReportsComponent>;
  let reportsService: ReportsService;

  // Mock the ReportsService and CobrandingService
  const reportsServiceMock = {
    getReports: jasmine.createSpy('getReports').and.returnValue(of({} as Reports)),
  };

  const cobrandingServiceMock = {};

  beforeEach(async () => {
    await TestBed.configureTestingModule({
      declarations: [ReportsComponent],
      providers: [
        { provide: ReportsService, useValue: reportsServiceMock },
        { provide: CobrandingService, useValue: cobrandingServiceMock },
      ],
    }).compileComponents();
  });

  beforeEach(() => {
    fixture = TestBed.createComponent(ReportsComponent);
    component = fixture.componentInstance;
    reportsService = TestBed.inject(ReportsService);
    fixture.detectChanges();
  });

  it('should create', () => {
    expect(component).toBeTruthy();
  });

  it('should fetch reports data on initialization', () => {
    const reportType = 'someType';
    component.ngOnInit();
    expect(reportsService.getReports).toHaveBeenCalledWith(reportType);
  });

  it('should set loadFailure$ to false when reports are fetched successfully', () => {
    component.fetchReportsData();
    expect(component.loadFailure$.getValue()).toBeFalse();
  });

  it('should set loadFailure$ to true when reports fetch fails', () => {
    const errorMessage = 'Failed to fetch reports data from the service';
    spyOn(console, 'error');
    spyOn(component.loadFailure$, 'next');

    reportsServiceMock.getReports.and.throwError(errorMessage);

    component.fetchReportsData();
    expect(console.error).toHaveBeenCalledWith('Failed to fetch reports data from the service:', errorMessage);
    expect(component.loadFailure$.next).toHaveBeenCalledWith(true);
  });

  it('should call fetchReportsData() when refresh() is called', () => {
    spyOn(component, 'fetchReportsData');
    component.refresh();
    expect(component.fetchReportsData).toHaveBeenCalled();
  });
});
