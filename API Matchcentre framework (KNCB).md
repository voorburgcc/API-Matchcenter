# Overview of Public ResultVault API URLs

This framework provides access to the KNCB (Royal Dutch Cricket Association) public API endpoints for cricket match data. It enables developers to build applications with that.

## Matchcentre Match URL

- **Scorecard:**  
    [`https://matchcentre.kncb.nl/match/134453-ENTITYID/scorecard/`](https://matchcentre.kncb.nl/match/134453-ENTITYID/scorecard/)

## API Endpoints

| Description                    | URL |
|--------------------------------|-----|
| KNCB Association               | [`https://api.resultsvault.co.uk/rv/134453/?apiid=1002`](https://api.resultsvault.co.uk/rv/134453/?apiid=1002) |
| Seasons                        | [`https://api.resultsvault.co.uk/rv/134453/seasons/?apiid=1002`](https://api.resultsvault.co.uk/rv/134453/seasons/?apiid=1002) |
| Grades                         | [`https://api.resultsvault.co.uk/rv/134453/grades/?apiid=1002&seasonId=19`](https://api.resultsvault.co.uk/rv/134453/grades/?apiid=1002&seasonId=19) |
| Matches (Pre-game Info / Squad)| [`https://api.resultsvault.co.uk/rv/134453/matches/?apiid=1002&seasonid=19&gradeid=GRADEID&action=ors&maxrecs=1000&strmflg=1`](https://api.resultsvault.co.uk/rv/134453/matches/?apiid=1002&seasonid=19&gradeid=GRADEID&action=ors&maxrecs=1000&strmflg=1) |
| Match Stream Highlights        | [`https://api.resultsvault.co.uk/rv/134453/matches/ENTITYID/?apiid=1002&strmflg=3`](https://api.resultsvault.co.uk/rv/134453/matches/ENTITYID/?apiid=1002&strmflg=3) |
| Ball-by-Ball                   | [`https://api.resultsvault.co.uk/rv/134453/matches/ENTITYID/?apiid=1002&action=getballs&sportid=1&resultid=RESULTID&inningsnumber=1`](https://api.resultsvault.co.uk/rv/134453/matches/ENTITYID/?apiid=1002&action=getballs&sportid=1&resultid=RESULTID&inningsnumber=1) |
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

---

## 2025 Grade ID Mapping

| grade_id | grade_name                |
|----------|--------------------------|
| 71374    | Topklasse                |
| 71375    | Hoofdklasse              |
| 73934    | Eerste Klasse            |
| 71378    | Topklasse Twenty20       |
| 82134    | Hoofdklasse Twenty20     |
| 82135    | Eerste Klasse Twenty20   |
| 73940    | Tweede Klasse            |
| 73941    | Derde Klasse             |
| 73942    | Vierde Klasse            |
| 73945    | Lagere Klassen Twenty20  |
| 82349    | Vrouwen Topklasse        |
| 75993    | Vrouwen T20              |
| 82351    | Vrouwen Hoofdklasse      |
| 73943    | Zami                     |
| 73944    | Zomi                     |
| 75943    | U17                      |
| 75913    | U15                      |
| 75914    | U13                      |
| 75916    | U11 B                    |
| 79816    | Bedrijfscompetitie       |

- The `grade_id` can also be found at:  
    [`/grades/?apiid=1002&seasonId=19`](https://api.resultsvault.co.uk/rv/134453/grades/?apiid=1002&seasonId=19)
- The `entity_id` can be found at:  
    [`/matches/?apiid=1002&seasonid=19&gradeid={{GRADE_ID}}&action=ors&maxrecs=1000&strmflg=1`](https://api.resultsvault.co.uk/rv/134453/matches/?apiid=1002&seasonid=19&gradeid=GRADEID&action=ors&maxrecs=1000&strmflg=1) (per grade)

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

- [`/matches/ENTITYID/?apiid=1002&strmflg=3`](https://api.resultsvault.co.uk/rv/134453/matches/ENTITYID/?apiid=1002&strmflg=3)
- [`/matches/ENTITYID/?apiid=1002&action=getballs&sportid=1&resultid=RESULTID&inningsnumber=1`](https://api.resultsvault.co.uk/rv/134453/matches/ENTITYID/?apiid=1002&action=getballs&sportid=1&resultid=RESULTID&inningsnumber=1)