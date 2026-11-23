Trades: plumbing, hvac, electrical

# Home services: the business comes to the person

Read with `SKILL.md`. These rules are for a job at the person's home or property.

## The place

| Need | Why | If missing |
| --- | --- | --- |
| The trade or the problem | Picks the trade and service | Ask what is wrong |
| The ZIP of the job (or the city) | Results are ranked by where each business has worked | Ask where the job is (SKILL.md section 3 says when not to ask) |
| How urgent | Emergencies skip the questions | Assume normal unless the words say otherwise |

The ZIP is where the job is, not where the person is now. "I'm at work, the leak is at home in 78745": use 78745.

## Emergencies

Signals: water pouring or a burst pipe, sewage backing up, no heat in freezing weather, no AC in extreme heat for
someone vulnerable, sparks, smoke, a burning smell from wiring, a gas smell.

- **Gas smell:** first tell them to leave the building now and, from outside, call 911 and the gas company. Then help
  find a plumber.
- **Sparks, smoke or fire:** call 911 first.
- Otherwise: ask only for the place if you do not have it, search (`limit=3`), and give 2 or 3 phone numbers in the
  first answer. Keep it short.
- One safety step is fine: for water, the home's main water valve; for electrical, the breaker. Do not give
  repair instructions.

## Reading and answering

- **Main action: call.** Put the phone number first. The person does not need the business's address: it comes to
  them.
- **Trust: license and permits.** `zip_match: job` means it has done permitted work in that ZIP: say it ("14
  permits nearby in the last year" when `why` says so). Licenses as in SKILL.md section 5.
- **Next step:** "call them and mention the leaking water heater"; suggest asking for their license number when the
  license is not verified.

## Examples (made-up businesses)

**Normal**
> Person: My water heater is leaking. I'm in 78704.
> Calls: `list_services(trade=plumbing)` -> `find_businesses(trade=plumbing, service=plumbing.water_heater_repair,
> zip=78704)` (`fallback: "trade"`: plumbers for 78704) -> `get_business` on the top 3.
> Answer: "Here are three plumbers for 78704, in the directory's order. None has water heater repair confirmed
> yet, so ask when you call. 1. Example Creek Plumbing - (512) 555-0101. Why: plumbing license current, 14 permits
> nearby in the last year. Note: phone not confirmed by a second source. 2. ... Hours and prices aren't known: call
> to ask. Until someone arrives, you can turn off the cold-water valve on top of the heater."

**Emergency**
> Person: Pipe burst, water everywhere, 78728, help
> Calls: `find_businesses(trade=plumbing, zip=78728, limit=3)` -> `get_business` on the top 2 or 3.
> Answer: "Turn off your main water valve now if you can. Call one of these: 1. Example Valley Plumbing - (512)
> 555-0102 ... 2. ... 3. ... I don't know who is open right now, so call each until one answers."

**City, no ZIP needed**
> Person: I need an electrician in Example City.
> Calls: `find_businesses(trade=electrical, city="Example City")` -> `get_business` on the top 3. No question about
> the ZIP.
