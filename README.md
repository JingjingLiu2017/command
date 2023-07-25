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
