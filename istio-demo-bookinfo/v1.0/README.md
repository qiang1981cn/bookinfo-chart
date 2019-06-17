# Bookinfo

[Bookinfo](https://istio.io/docs/examples/bookinfo/) deploys a sample application composed of four separate microservices used to demonstrate various Istio features. The application displays information about a book, similar to a single catalog entry of an online book store. Displayed on the page is a description of the book, book details (ISBN, number of pages, and so on), and a few book reviews.

The Bookinfo application is broken into four separate microservices:

* productpage. The productpage microservice calls the details and reviews microservices to populate the page.
* details. The details microservice contains book information.
* reviews. The reviews microservice contains book reviews. It also calls the ratings microservice.
* ratings. The ratings microservice contains book ranking information that accompanies a book review.

There are 3 versions of the reviews microservice:

* Version v1 doesn’t call the ratings service.
* Version v2 calls the ratings service, and displays each rating as 1 to 5 black stars.
* Version v3 calls the ratings service, and displays each rating as 1 to 5 red stars.

# How to use it?

## Prerequisites

* Enable Istio for this cluster
* Find a namespace and toggle on the sidecar auto-injection for it

## Depoly reviews v1 version only

* Find this book info app in App Launch page and click `View Details`.
* Select the namespace which has sidecar auto-injection enabled
* Set `Version` in `REVIEW SERVICE MASTER VERSION` to `v1`
* Select `False` for `Deploy Canary Version of Review Service`.
* Click the productpage nodeport and verify everthing works. You should be able to see the Book Details and the Book Reviews.

## Deploy reviews v1 and v2 versions at the same time

* Find the app in Apps page and click `Upgrade`
* Select `True` for `Deploy Canary Version of Review Service`. 
* Set `Version` in `REVIEW SERVICE CANARY VERSION` to `v2`
* Set `Weight` for Canary version to 0
* Set `Weight` for Master version to 100
* Set `Canary Test User` to jason
* Click `Upgrade`
* It will deploy related Istio Virtual Service and Destination Rule
* Now normal users can only still see review v1 version.
* Login as `jason`, then jason will be able to see the review v2 version. You can see the black stars if you login as jason

## Shift more traffics to reviews v2 for all users

* Find the app in Apps page and click `Upgrade`
* Set `Weight` for Canary version to 50
* Set `Weight` for Master version to 50
* Click `Upgrade`
* It will update related Istio Virtual Service
* Now normal users can see the black stars with a 50 percent chance

## Shift all traffics to reviews v2 version

* Find the app in Apps page and click `Upgrade`
* Select `False` for `Deploy Canary Version of Review Service`
* Set `Version` in `REVIEW SERVICE MASTER VERSION` to `v2`
* It will remove the reviews v1 workload and related Istio config
* Now all users can always see the black stars