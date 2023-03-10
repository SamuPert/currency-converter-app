# Cloud Architecture

This is a currency conversion app that allows users to convert between different currencies. The app is hosted on AWS and utilizes several AWS services to provide a seamless user experience.

![Cloud architecture](./resources/currency-converter-cloud-architecture.svg)

1. A lambda function called "UpdateExchangeRatesData" is scheduled to run daily at 4:30 PM CET.
This lambda function will be responsible for fetching the latest exchange rates data from the public API provided by https://ec.europa.eu/.
The lambda function will use the appropriate API endpoint to retrieve the data and parse it into a usable format.

2. The lambda "UpdateExchangeRatesData" stores the parsed exchange data in a DynamoDB record, called "ExchangeRates".

3. Users will use AWS Route 53 to resolve the domain name to access the app. When a user types in the domain name in their browser, Route 53 will route the request to the CloudFront CDN.

4. CloudFront is a content delivery network that is used to serve the app's content to the users. It caches static resources like HTML, CSS, and JavaScript files.

5. The static resources for the app will be stored in an S3 bucket. When a user requests a resource, CloudFront will first check if the resource is available in its cache. If the resource is not available, CloudFront will fetch it from the S3 bucket and cache it for future requests.

6. The API gateway is used to provide a RESTful API for the app. App users will interact with the API gateway to access app features. The API Gateway will route requests to the appropriate backend service, although there is still only one endpoint.

7. The API gateway will act as a service proxy to interact with the "ExchangeRates" database service. When a user requests a currency conversion, the API gateway will retrieve the latest exchange rates data from DynamoDB "ExchangeRates" record and perform the currency conversion. The result will be returned to the user.