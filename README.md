// shimmer.service.ts
import { Injectable } from '@angular/core';
import { BehaviorSubject } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class ShimmerService {
  // Using BehaviorSubject to store the current state of the shimmer
  private shimmerState = new BehaviorSubject<boolean>(false);

  // Observable to allow other components to subscribe to the shimmer state
  shimmerState$ = this.shimmerState.asObservable();

  constructor() { }

  // Function to show the shimmer
  showShimmer() {
    this.shimmerState.next(true);
  }

  // Function to hide the shimmer
  hideShimmer() {
    this.shimmerState.next(false);
  }
}

