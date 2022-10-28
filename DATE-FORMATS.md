# Date formatting

For any API calls and by default, Deep Intelligence uses the [ISO 8601](https://es.wikipedia.org/wiki/ISO_8601) standard format for dates.

For visualizations and source file parsing, you will be able to provide a custom format for dates. In this case, you have to input a text including the wildcards that will be replaced by the date and time components.

| Wildcard | Description |
|---|---|
| `YYYY` | Year |
| `MM` | Month. Represented by a number from 01 to 12. |
| `MMM` | Month. Represented by 3 character codes. Examples: `Jan`, `Feb`, `Nov`, `Dec` |
| `MMMM` | Month. Represented with its full name. Examples: `January`, `February`, `November`, `December` |
| `ww` | Week of the year |
| `DD` | Day of the month |
| `HH` | Hour of the day |
| `mm` | Minutes |
| `ss` | Seconds |
| `SSS` | Milliseconds |
| `Z` | Time zone |

Here is an example:

```
YYYY-MM-DD HH:mm:ss

2022-10-28 12:30:00
```
