---
theme: gaia
paginate: true
footer: "https://github.com/tc39/proposal-temporal"
style: |
  @import url('https://cdn.jsdelivr.net/npm/hack-font@3/build/web/hack-subset.css');
  code { font-family: Hack; }
  section { letter-spacing: 0; }
  ul ul li { font-size: 75%; }
---

<!-- _class: invert lead -->

# 🕓 Temporal

**Justin Grant** 
(invited expert)
TC39 May 2021

---

## Temporal 2021-05 Update

- Asking for consensus on 2 minor normative PRs
- What else is happening?
  - Working through SpiderMonkey implementer feedback ([#1502](https://github.com/tc39/proposal-temporal/issues/1502)) 
  - Porting [docs](https://tc39.es/proposal-temporal/docs) to MDN ([#1449](https://github.com/tc39/proposal-temporal/issues/1449))
  - Building more Test262 tests

---

## [#1510](https://github.com/tc39/proposal-temporal/pull/1510) Normative: Fix inconsistent property names/order returned by getISOFields() 
- Expected: (for objects returned by all `getISOFields()` methods)
  - Unit property names prefixed with `iso`
  - Properties inserted in alphabetical order
  - e.g. `{ calendar: 'chinese', isoDay: 25, isoMonth: 5, isoYear: 2021 }`
- Actual: (spec bugs, not intended)
  - `ZonedDateTime.p.getISOFields()` result lacks the `iso` prefix
  - `PlainDateTime.p.getISOFields()` result has the wrong order

---

## [#1509](https://github.com/tc39/proposal-temporal/pull/1509) Normative: Use singular unit names throughout (for `Intl` compatibility)
```javascript
// Intl: unit name *output* is always singular, so singular preferred in docs
//       unit name *input* accepts singular or plural
dtf = new Intl.DateTimeFormat('en-US');
dtf.formatToParts(new Date())[2]; // {type: "day", value: "14"}
new Intl.RelativeTimeFormat('en-US').format(1, 'weeks'); // "in 1 week"
new Intl.RelativeTimeFormat('en-US').format(1, 'week');  // "in 1 week"

// Temporal: unit name *output* (custom calendars only) can be plural 
//           unit name *input* accepts singular or plural, EXCEPT 'week'
date = Temporal.PlainDate.from('2021-05-14');
date.until('2021-12-31', {smallestUnit: 'weeks'}); // P33W 👍
date.until('2021-12-31', {smallestUnit: 'week'});  // RangeError ❌
```

---

## [#1509](https://github.com/tc39/proposal-temporal/pull/1509) Normative: Use singular unit names throughout (cont'd)
  - Changes
    - Also accept `'week'` for unit names in Temporal options objects
    - Only pass singular unit names to userland code in custom `Calendar.p.dateUntil()`
    - Change docs to prefer singular unit names (like Intl docs)
  - Only affects string values in options objects
    - `Temporal.Duration` properties like `d.weeks` stay plural (like other date/time APIs)

---

## Consensus for these changes?

- [#1510](https://github.com/tc39/proposal-temporal/pull/1510) Normative: Fix inconsistent property names/order returned by `getISOFields()` 
- [#1509](https://github.com/tc39/proposal-temporal/pull/1509) Normative: Use singular unit names throughout
