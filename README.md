[Airbnb](https://apify.com/scrapestorm/AirBNB?fpr=data)

## Airbnb Reviews Scraper 💬

The Airbnb Reviews Scraper is a tool designed to extract review data from specific Airbnb listings. By providing an Airbnb listing URL, you can retrieve:

- All reviews for the specified Airbnb listing
- Details such as review text, reviewer name, review date, rating, and more
- Additional information about each reviewer, including whether they are a superhost and other relevant attributes

## How to Use the Airbnb Reviews Scraper 💬,

- Input the Airbnb Listing  id_room: Enter the Airbnb id offer that you want to scrape reviews from.
- Click Start: Begin the scraping process by clicking the "Start" button. The scraper will navigate the listing and collect reviews data.
- Download Your Data: Once the extraction is complete, download your results in the format that best suits your needs, such as JSON, XML, CSV, Excel, or HTML.

## Pricing 💰

This scraper operates on a pay-per-result basis at a cost of $13.99/month.

## Related Actors

If you're interested in specific specialties, such as real estate agents, photographers, inspectors, property managers, or home improvement scraping solutions, check out these related actors:

- [AirBNB reviews scraper (Fast & cheap) 📈](https://apify.com/scrapestorm/airbnb)
- [AirBNB scraper (⭐ Fast & cheap) - Rental](https://apify.com/scrapestorm/airbnb-scraper-fast-cheap---rental)
- [Facebook Group Members Scraper 📊👥 - Fast & cheap 💬⭐](https://apify.com/scrapestorm/my-actor-1)
- [Facebook Ads Library Scraper 🎯📈 - Fast & cheap 💬⭐](https://apify.com/scrapestorm/facebook-ads-library-scraper---fast-cheap)
- [Facebook Followers & Following Scraper 📊👥 - Fast & cheap 💬⭐](https://apify.com/scrapestorm/facebook-followers-following-scraper---fast-cheap)
- [Zillow (Find a Real Estate Agent) 🏡](https://apify.com/scrapestorm/zillow-find-a-real-estate-agent)
- [Zillow (Find a Property Managers) 🏢👨‍💼👩‍💼](https://apify.com/scrapestorm/zillow-find-a-property-managers)
- [Zillow (Find a Home Improvement Agent) 🛠️)](https://apify.com/scrapestorm/zillow-find-a-home-improvement-agent)
- [Zillow (Find a Home Inspector Agent) 🔍](https://apify.com/scrapestorm/zillow-find-a-home-inspector-agent)
- [Zillow (Find a Photographer Agent) 📸](https://apify.com/scrapestorm/zillow-find-a-photographer-agent)
- [Facebook Group Members Scraper 📊👥 - Fast & cheap 💬⭐](https://apify.com/scrapestorm/my-actor-1)
- [Facebook Ads Library Scraper 🎯📈 - Fast & cheap 💬⭐](https://apify.com/scrapestorm/facebook-ads-library-scraper---fast-cheap)
- [Facebook Followers & Following Scraper 📊👥 - Fast & cheap 💬⭐](https://apify.com/scrapestorm/facebook-followers-following-scraper---fast-cheap)
- [PropertyFinder Search Scraper 🏡 🏘️ - Cheap](https://apify.com/scrapestorm/propertyfinder-search-scraper---cheap)
- [PropertyFinder Agent - Cheap 🏷️ 🏠👨‍💼](https://apify.com/scrapestorm/propertyfinder-agent---cheap)
- [🛍️ Dubizzle Search Scraper - Cheap 🏷️](https://apify.com/scrapestorm/dubizzle-search-scraper---cheap)
- [Zillow Agent Scraper (All-in-one) 🏡📧📞🤖](https://apify.com/scrapestorm/zillow-agent-scraper-all-in-one)
- [Facebook Comments Scraper (All-in-One) 💬](https://apify.com/scrapestorm/facebook-comments-scraper-all-in-one)
- [Facebook Likes Scraper (Fast & Cheap) 👍 🌟](https://apify.com/scrapestorm/facebook-likes-scraper-fast-cheap)
- [Pinterest Profiles Search Scraper 🖼️👥🔍](https://apify.com/scrapestorm/pinterest-profiles-search-scraper)
- [Pinterest Boards Search Scraper 🎨🔍](https://apify.com/scrapestorm/pinterest-boards-search-scraper)
- [Pinterest Pins/Videos Search Scraper 🎨🔍](https://apify.com/scrapestorm/pinterest-pins-videos-search-scraper)
- [YouTube Scraper (By Keyword) - ⭐⭐⭐⭐⭐ Fast & cheap](https://apify.com/scrapestorm/youtube)
- [YouTube Scraper (By Keyword) - ⭐ Fast & cheap - Result](https://apify.com/scrapestorm/youtube-scraper-by-keyword---fast-cheap---resultl)
- [Youtube Playlist Scraper 🎵 - Rental (Fast & cheap)](https://apify.com/scrapestorm/youtube-playlist-scraper---rental-fast-cheap)

---

## Airbnb Reviews Scraper Input 🔑

The Airbnb Reviews Scraper requires a specific input format:
a JSON object with the URL of the Airbnb listing you wish to scrape.

```
{
  "id_room": [
    "49312153"
  ]
}
```

## Airbnb Reviews Scraper Data Output 📊

The Airbnb Reviews Scraper will output the data to the Apify dataset. After the run is finished,
you can download the dataset in various formats, including JSON, CSV, XML, RSS, and HTML Table.

```
{
        "id_comment": "1277449383601058355",
        "comment": "Was very good",
        "language": "en",
        "createdAt": "2024-10-28T13:03:53Z",
        "localizedDate": "2 weeks ago",
        "rating": 5,
        "reviewer_firstName": "Anass",
        "reviewer_id": "522093223",
        "reviewer_pictureUrl": "https://a0.muscache.com/im/pictures/user/User/original/65567776-25f8-4176-8f63-776a1a612842.jpeg",
        "reviewee_firstName": "Residhome",
        "reviewee_id": "397836098",
        "reviewee_pictureUrl": "https://a0.muscache.com/im/pictures/user/57800a2f-3fb2-4bf9-a1a4-45822dda6202.jpg",
        "reviewee_isSuperhost": false,
        "reviewHighlight": "Stayed a few nights",
        "highlightType": "LENGTH_OF_STAY"
    }
```