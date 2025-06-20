# ResultVault API Framework (Generic)

## Introduction

This generic framework demonstrates how to interact with ResultVault API endpoints for cricket data access. It serves as a template for accessing cricket match information from any ResultVault-powered cricket association.

**What this framework enables:**
- **Real-time cricket match data access** without API authentication
- **League and competition browsing** with automated data retrieval
- **Match detail extraction** including scores, teams, and venues
- **Ball-by-ball data access** for detailed match analysis
- **Video stream integration** for live match content

**Practical Applications:**
- **Cricket association websites** needing live match data display
- **Third-party cricket apps** requiring competition data feeds
- **Sports betting platforms** needing real-time cricket information
- **Cricket analytics tools** for statistical analysis
- **Fan engagement platforms** with live score integration
- **Media websites** displaying cricket results and fixtures

**Framework Components:**
- HTML demonstration pages showing league overviews and match listings
- JavaScript functions for API data retrieval and display
- Error handling and retry mechanisms for reliable data access
- CORS workarounds for browser-based API consumption

This framework can be adapted for any cricket association using the ResultVault platform by updating the platform ID and API parameters.

---

# Overview of Public ResultVault API URLs

## Matchcentre Match URL

- **Scorecard:**  
    [`https://matchcentre.kncb.nl/match/134453-{{entity_id}}/scorecard/`](https://matchcentre.kncb.nl/match/134453-{{entity_id}}/scorecard/)

## API Endpoints

| Description                    | URL |
|--------------------------------|-----|
| KNCB Association               | [`https://api.resultsvault.co.uk/rv/134453/?apiid=1002`](https://api.resultsvault.co.uk/rv/134453/?apiid=1002) |
| Seasons                        | [`https://api.resultsvault.co.uk/rv/134453/seasons/?apiid=1002`](https://api.resultsvault.co.uk/rv/134453/seasons/?apiid=1002) |
| Grades                         | [`https://api.resultsvault.co.uk/rv/134453/grades/?apiid=1002&seasonId=19`](https://api.resultsvault.co.uk/rv/134453/grades/?apiid=1002&seasonId=19) |
| Matches (Pre-game Info / Squad)| [`https://api.resultsvault.co.uk/rv/134453/matches/?apiid=1002&seasonid=19&gradeid=GRADEID&action=ors&maxrecs=1000&strmflg=1`](https://api.resultsvault.co.uk/rv/134453/matches/?apiid=1002&seasonid=19&gradeid=GRADEID&action=ors&maxrecs=1000&strmflg=1) |
| Match Stream Highlights        | [`https://api.resultsvault.co.uk/rv/134453/matches/{{entity_id}}/?apiid=1002&strmflg=3`](https://api.resultsvault.co.uk/rv/134453/matches/{{entity_id}}/?apiid=1002&strmflg=3) |
| Ball-by-Ball                   | [`https://api.resultsvault.co.uk/rv/134453/matches/{{entity_id}}/?apiid=1002&action=getballs&sportid=1&resultid=RESULTID&inningsnumber=1`](https://api.resultsvault.co.uk/rv/134453/matches/{{entity_id}}/?apiid=1002&action=getballs&sportid=1&resultid=RESULTID&inningsnumber=1) |
| Breaks                         | [`https://api.resultsvault.co.uk/rv/lookup/?apiid=1002&sportid=1&lookupid=MATCH_BREAK_TYPES`](https://api.resultsvault.co.uk/rv/lookup/?apiid=1002&sportid=1&lookupid=MATCH_BREAK_TYPES) |

> **Note:** These API URLs are public and do not require an API token.

---

## Key Parameters

| Parameter         | Value / Description              |
|-------------------|---------------------------------|
| `134453`          | KNCB platform ID                 |
| `apiid=1002`      | KNCB contact                     |
| `seasonId=19`     | Year 2025                        |
| `inningsnumber=1` | Innings number (1 or 2)          |
at least one result_id found: 24965458
---

## grade_id and entity_id Mapping

- Find the grades (leagues) via `grade_id` in:  
    [`https://api.resultsvault.co.uk/rv/134453/grades/?apiid=1002&seasonId=19`](https://api.resultsvault.co.uk/rv/134453/grades/?apiid=1002&seasonId=19)
- The `entity_id` can be found at:  
    [`/matches/?apiid=1002&seasonid=19&gradeid={{grade_id}}&action=ors&maxrecs=1000&strmflg=1`](https://api.resultsvault.co.uk/rv/134453/matches/?apiid=1002&seasonid=19&gradeid=grade_id&action=ors&maxrecs=1000&strmflg=1) (per grade where  grade_id from the url with grades/leagues mentioned above)

---

## Match Object Key Properties

```js
{
    match_id: string,       // Match ID
    date1: string,          // Microsoft JSON date format
    grade_name: string,     // League/Competition name
    status_id: number,      // 30=in progress, 60=completed
    home_name: string,      
    away_name: string,      
    score_text: string,     
    leader_text: string,    
    venue_name: string,     
    matchStreams: Array,    // Contains video_id if stream available
    MatchTeams: Array,      // Detailed team info including entity_id
    Round: number           
}
```
There are many more Match object key properties that you will find when exploring the API urls.

---

## Important Usage Note

Some URLs must be opened shortly before use to generate data via JavaScript; otherwise, the API call may return an error:

- [`/matches/{{entity_id}}/?apiid=1002&strmflg=3`](https://api.resultsvault.co.uk/rv/134453/matches/{{entity_id}}/?apiid=1002&strmflg=3)
- [`/matches/{{entity_id}}/?apiid=1002&action=getballs&sportid=1&resultid={{RESULTID}}&inningsnumber=1`](https://api.resultsvault.co.uk/rv/134453/matches/{{entity_id}}/?apiid=1002&action=getballs&sportid=1&resultid={{RESULTID}}&inningsnumber=1)
