Trades: salon, barber

# Visit businesses: the person goes to the business

Read with `SKILL.md`. These rules are for a business the person visits, such as a salon or a barbershop.

## The place

| Need | Why | If missing |
| --- | --- | --- |
| The service ("highlights", "a fade", "gel nails") | Picks the trade and service | Ask what they want done |
| Where the person is (home, work, or where they'll be) | They go there, so distance matters | Ask: "Which area are you in?" (a ZIP, a city or a neighbourhood). SKILL.md section 3 says when not to ask |

- Hair, nails, brows, lashes, waxing, facials, makeup: `salon`. Men's cut, fade, beard trim, shave: `barber`.
- A men's haircut can be either: search `barber` first, unless they say salon; you may offer salons too.
- `list_services(trade=salon)` or `(trade=barber)` gives the service codes and their synonyms.

## Reading and answering

- **Main action: book online.** Give the `book_online` link first when there is one, then the phone.
- **The address.** They go there, so give the street when the profile shows one (`storefront: true`). Otherwise only
  the city and ZIP: "call for the address" (home salons and salon suites keep the street hidden).
- **Distance.** For `nearby` cards say "in 78704, about 4 km away".
- **Trust: license, booking, hours.** There are no permits for these trades, so `why` names the license, the booking
  page or the sources instead. A TDLR establishment license counts only when verified.
- **Hours and prices** only from the profile: prices from `item_prices` are what the website lists ("their site
  lists women's haircuts from $65"). Otherwise "call to ask".
- **Never promise** a stylist, a free time, a price or walk-ins. You cannot see free slots: the booking page shows
  them.

## Examples (made-up businesses)

**Salon**
> Person: I need highlights somewhere near 78745, ideally I can book online.
> Calls: `list_services(trade=salon)` -> `find_businesses(trade=salon, service=salon.highlights, zip=78745)` ->
> `get_business` on the top 3.
> Answer: "Three salons near 78745, in the directory's order: 1. Example Hair Studio - book online: <link>, (512)
> 555-0301. Why: TDLR license current, books online. It's in 78704, about 4 km away; address 300 Example Blvd Ste 2.
> 2. ... I can't see free times; the booking page shows them."

**Barber**
> Person: Where can I get a fade near 78702?
> Calls: `find_businesses(trade=barber, zip=78702)` -> `get_business` on the top 3.
> Answer: list them in order with the booking link if any and the phone; "call to ask about walk-ins".

**No ZIP yet**
> Person: Where can I get a gel manicure?
> Answer, no tool call: "Sure - which area are you in? A ZIP, city or neighbourhood is enough."
