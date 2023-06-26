import { getTranslocoTestingModule } from '@testing/transloco.module.spec';
import { UserDetailsService } from '@features/services/user-details/user-details.service';
import { USER_DETAILS } from '@mock/collections-index';
import { UserDetails } from '@features/services/user-details/user-details.model';
import { HttpClientTestingModule } from '@angular/common/http/testing';
import { CardholderDashboardComponent } from '@features/cardholder-dashboard/cardholder-dashboard.component';
import { ComponentFixture, TestBed } from '@angular/core/testing';
import { AccountInfoComponent } from '@features/widgets/account-info/account-info.component';
import { ResourceCenterComponent } from '@features/widgets/resource-center/resource-center.component';
import { NewsComponent } from '@features/widgets/news/news.component';
import { QuickLinksCardComponent } from '@shared/components/quick-links-card/quick-links-card.component';
import { RouterTestingModule } from '@angular/router/testing';

describe('CardholderDashboardComponent', () => {
  let component: CardholderDashboardComponent;
  let fixture: ComponentFixture<CardholderDashboardComponent>;
  let service: UserDetailsService;

  beforeEach(async () => {
    await TestBed.configureTestingModule({
      imports: [HttpClientTestingModule, getTranslocoTestingModule(), RouterTestingModule],
      declarations: [
        CardholderDashboardComponent,
        AccountInfoComponent,
        ResourceCenterComponent,
        NewsComponent,
        QuickLinksCardComponent
      ]
    }).compileComponents();
  });

  beforeEach(() => {
    fixture = TestBed.createComponent(CardholderDashboardComponent);
    component = fixture.componentInstance;
    service = TestBed.inject(UserDetailsService);
    fixture.detectChanges();
  });

  it('should create', () => {
    expect(component).toBeTruthy();
  });

  it('should return expected display name', () => {
    service.getUserDetailsByEntityGuid('').subscribe((userDetailResponse: UserDetails) => {
      expect(userDetailResponse.displayName).toEqual(USER_DETAILS.displayName);
      expect(component.displayName).toEqual(USER_DETAILS.displayName);
    });
  });
});

CardholderDashboardComponent > should create
NullInjectorError: R3InjectorError(DynamicTestModule)[WindowRef -> WindowRef]: 
  NullInjectorError: No provider for WindowRef!
CardholderDashboardComponent > should return expected display name
NullInjectorError: R3InjectorError(DynamicTestModule)[WindowRef -> WindowRef]: 
  NullInjectorError: No provider for WindowRef!
import { Component, Injector, Input, OnInit } from '@angular/core';
import { UserDetailsService } from '@features/services/user-details/user-details.service';
import { Subscription } from 'rxjs';

@Component({
  selector: 'app-ch-dashboard',
  templateUrl: './cardholder-dashboard.component.html',
  styleUrls: ['./cardholder-dashboard.component.scss']
})
export class CardholderDashboardComponent implements OnInit {
  isIccpUser = true;
  displayName: string;
  private readonly userDetailsService: UserDetailsService;
  @Input() guid: string;

  constructor(private readonly injector: Injector) {
    this.userDetailsService = injector.get(UserDetailsService);
  }
  ngOnInit(): void {
    this.userDetailsService
      .getUserDetailsByEntityGuid(this.guid)
      .subscribe(dashboardRes => {
        this.displayName = dashboardRes.displayName;
      });
  }
}
