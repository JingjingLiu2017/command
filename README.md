import { TestBed, ComponentFixture } from '@angular/core/testing';
import { ReportsComponent } from './reports.component';
import { of, throwError } from 'rxjs';
import { ReportsService } from '@features/services/reports/reports.service';
import { CobrandingService } from '@shared/services/cobranding/cobranding.service';
import { Reports } from '@features/services/reports/reports.model';
import { QuickLinkCard } from '@shared/components/quick-links-card/quick-link-card.model';

describe('ReportsComponent', () => {
  let component: ReportsComponent;
  let fixture: ComponentFixture<ReportsComponent>;
  let reportsService: ReportsService;
  let cobrandingService: CobrandingService;

  beforeEach(async () => {
    await TestBed.configureTestingModule({
      declarations: [ ReportsComponent ],
      providers: [ 
        { provide: ReportsService, useValue: jasmine.createSpyObj('ReportsService', ['getReports']) }, 
        { provide: CobrandingService, useValue: jasmine.createSpyObj('CobrandingService', ['']) }
      ]
    }).compileComponents();
    fixture = TestBed.createComponent(ReportsComponent);
    component = fixture.componentInstance;
    reportsService = TestBed.inject(ReportsService);
    cobrandingService = TestBed.inject(CobrandingService);
  });

  it('should create the component', () => {
    expect(component).toBeTruthy();
  });

  describe('fetchReportsData', () => {
    it('should fetch data successfully', () => {
      const reports = { } as Reports;
      (reportsService.getReports as jasmine.Spy).and.returnValue(of(reports));

      component.fetchReportsData();
      expect(reportsService.getReports).toHaveBeenCalled();
    });

    it('should handle error when fetching data', () => {
      const error = { message: 'Error' };
      (reportsService.getReports as jasmine.Spy).and.returnValue(throwError(error));

      component.fetchReportsData();
      expect(reportsService.getReports).toHaveBeenCalled();
    });
  });
  
  describe('refresh', () => {
    it('should refresh reports data', () => {
      const spy = spyOn(component, 'fetchReportsData');
      component.refresh();
      expect(spy).toHaveBeenCalled();
    });
  });
});
