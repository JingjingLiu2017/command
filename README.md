import { HttpClient } from '@angular/common/http';
import { Injectable } from '@angular/core';
import { Observable, finalize } from 'rxjs';
import { News } from './news.model';
import { ShimmerService } from '@shared/services/shimmer/shimmer.service';

@Injectable({
  providedIn: 'root'
})
export class NewsService {
  constructor(private readonly http: HttpClient, private shimmerService: ShimmerService) {}

  private readonly url = '/sdng/portalservice/retrieveNews.json';

  getNews(): Observable<News> {
    this.shimmerService.startLoading();

    return this.http.get<News>(this.url).pipe(
      finalize(() =>{
        this.shimmerService.stopLoading();
      })
    );
  }
}
