How should I update report-mock.service.ts: import { RequestInfo, STATUS } from 'angular-in-memory-web-api';
import { REPORTS } from './reports.stub';

export class NewsMockService {
  patternCollectionMap = [
    {
      pattern: /^\/sdng\/portalservice\/retrieveReportsDataFilesAndStatements.json/,
      collectionName: 'REPORTS',
      get: this.getReports.bind(this)
    }
  ];
  private getReports(reqInfo: RequestInfo) {
    return reqInfo.utils.createResponse$(() => ({
      body: REPORTS,
      headers: reqInfo.headers,
      status: STATUS.OK
    }));
  }

  private getServiceErrorResponse(reqInfo, errorResponse, status) {
    return {
      error: errorResponse,
      headers: reqInfo.headers,
      status: status
    };
  }
}
for the above changes


  private getReports(reqInfo: RequestInfo) {
    const urlSearchParams = new URLSearchParams(reqInfo.req.url.split('?')[1]);
    const status = urlSearchParams.get('status');

    if (status === 'COMPLETED' || status === 'SCHEDULED') {
      const filteredReports = REPORTS.filter(report => report.status === status);
      return reqInfo.utils.createResponse$(() => ({
        body: filteredReports,
        headers: reqInfo.headers,
        status: STATUS.OK
      }));
    } else {
      // If the status is not specified or invalid, return all reports
      return reqInfo.utils.createResponse$(() => ({
        body: REPORTS,
        headers: reqInfo.headers,
        status: STATUS.OK
      }));
    }
  }
