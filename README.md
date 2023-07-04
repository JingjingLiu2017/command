NewsService > should start and stop shimmer loading while fetching news
Expected spy ShimmerService.stopLoading to have been called.
Error: Expected spy ShimmerService.stopLoading to have been called.
    at <Jasmine>
    at Object.next (http://localhost:9876/_karma_webpack_/webpack:/src/app/features/services/news/news.service.spec.ts:83:45)
    at ConsumerObserver.next (http://localhost:9876/_karma_webpack_/webpack:/node_modules/rxjs/dist/esm/internal/Subscriber.js:91:1)
    at SafeSubscriber._next (http://localhost:9876/_karma_webpack_/webpack:/node_modules/rxjs/dist/esm/internal/Subscriber.js:60:1)
NewsService > should handle errors while fetching news
Expected HttpErrorResponse({ headers: HttpHeaders({ normalizedNames: Map(  ), lazyUpdate: null, headers: Map(  ) }), status: 500, statusText: 'Internal Server Error', url: '/sdng/portalservice/retrieveNews.json', ok: false, name: 'HttpErrorResponse', message: 'Http failure response for /sdng/portalservice/retrieveNews.json: 500 Internal Server Error', error: 'Error fetching news.' }) to equal 'Error fetching news.'.
Error: Expected HttpErrorResponse({ headers: HttpHeaders({ normalizedNames: Map(  ), lazyUpdate: null, headers: Map(  ) }), status: 500, statusText: 'Internal Server Error', url: '/sdng/portalservice/retrieveNews.json', ok: false, name: 'HttpErrorResponse', message: 'Http failure response for /sdng/portalservice/retrieveNews.json: 500 Internal Server Error', error: 'Error fetching news.' }) to equal 'Error fetching news.'.
    at <Jasmine>
    at Object.error (http://localhost:9876/_karma_webpack_/webpack:/src/app/features/services/news/news.service.spec.ts:100:23)
    at ConsumerObserver.error (http://localhost:9876/_karma_webpack_/webpack:/node_modules/rxjs/dist/esm/internal/Subscriber.js:102:1)
    at SafeSubscriber._error (http://localhost:9876/_karma_webpack_/webpack:/node_modules/rxjs/dist/esm/internal/Subscriber.js:64:1)
Expected spy ShimmerService.stopLoading to have been called.
Error: Expected spy ShimmerService.stopLoading to have been called.
    at <Jasmine>
    at Object.error (http://localhost:9876/_karma_webpack_/webpack:/src/app/features/services/news/news.service.spec.ts:102:47)
    at ConsumerObserver.error (http://localhost:9876/_karma_webpack_/webpack:/node_modules/rxjs/dist/esm/internal/Subscriber.js:102:1)
    at SafeSubscriber._error (http://localhost:9876/_karma_webpack_/webpack:/node_modules/rxjs/dist/esm/internal/Subscriber.js:64:1
