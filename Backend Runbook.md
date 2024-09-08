# Backend Runbook

This is for backend developers on Immersion Day. We aim to guide you through the development process, including fixing bugs and adding features within a limited time. You can follow this guide step by step.

## First Bug - Turn UP RATIO(%) 6 month is missing

### Problem

When we click 6M in the TURN UP RATIO (%) graph, we see that the last bar chart is missing. We need to fix this by ensuring it returns the correct data.
![Bug - Turnup ratio 6 month missing](./assets/backend-runbook\bug1-turnup-ratio-6month-missing.png)

### Investigation

We need to check why the data is missing. Is the API on the backend returning data correctly, or is the frontend manipulating the data and returning it incorrectly? We should check the network request tab for more information.

1. Open chrome, and open DevTools - Chrome for Developers(Ctrl+shift+I) and go to network request tab.
2. Navigate to https://lseg-th.github.io/travelogo-dashboard/
3. The network request should be shown. Look for `sixMonthsTurnupRatioData`
4. We will see that the data is returning only an array of 5 items instead of 6. This means the data returned from the backend API is not correct.
   ![Bug - Turnup ratio 6 month missing](./assets/backend-runbook\bug1-data-return-incorrect.png)

### Start Fixing

1. Navigate to `routeApi.ts`
2. Check `routerApi.get('/v1/sixMonthsTurnupRatioData', getSixMonthsTurnupRatioData);`
3. Click in `getSixMonthsTurnupRatioData` and it will navigate to `controllerTurnupRatio.ts`
4. you will see json respose with data in constant, and data have `FIXME:` so you can change to the data there to make it return correctly

![Bug - Turnup ratio 6 month missing](./assets/backend-runbook\bug1-fixing-incorrect-data.png)

## First Bug - Get Seasonal booking chart and Seasonal checkin chart

We need to improve the API response as well. We agree that on the frontend side, instead of sending the graph data, we should return only the raw data. The frontend will then handle and format it as needed.<br>
also, we need to return [HTTP Status Codes]("https://dev.to/_staticvoid/the-complete-guide-to-status-codes-for-meaningful-rest-apis-1-5c5") by using `statusCode` as well for make it align with REST API standard.

we will create the `/getSeasonalDataOneYear` endpoint, then frontend can re-use it

### Example Response

```json
{
  "data":{
    "booking": [10, 20, 30, 40, 50, 60],
    "checkin": [10, 20, 30, 40, 50, 60]
  }
  "statusCode": "200"
}
```
