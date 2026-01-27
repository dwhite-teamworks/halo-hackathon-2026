# HALO Hackathon 2026
 
This repository hosts the instructions provided by Teamworks and Sportlogiq for the HALO (Hockey Analytics League Operations) Hackathon.  The dataset is intended for use by registered hackathon participants to explore hockey performance, tactics, and decision-making through data-driven analysis.

## Data

The data made available for this competition has been curated specifically for the event. The datasets represent a subset of professional hockey data adapted for research and public use to ensure relevance and practical application. 

The data provided is tracking-enhanced event data from 480 games in the 2023-24 AHL season. We've combined event data (e.g., faceoffs, passes, puck recoveries, dump-ins) along with player location data at the moment of the event. Note that since tracking data is derived from broadcast video, location data is not necessarily available for every player on the ice at the time of the event.

Please note that the dataset will only be available to registered hackathon participants. If you are interested in participating - please register here: https://www.sportlogiq.com/halo-hackathon/

The data is available in five separate datasets:

- `games`: one record per game with team and outcome information
- `events`: one record per event 
- `stints`: one record per player per stint (a stretch of play during which a set of distinct players is on the ice)
- `players`: one record per player
- `tracking`: one record per player per event

The data files can be joined together via the `game_id`, `player_id`, and `sl_event_id` fields, which are the unique identifiers for each game, each player, and each event within a game, respectively.

The data is provided in the parquet file format for efficiency. Both R and Python packages offer functionality for reading parquet files; as a starting point we recommend `arrow::read_parquet()` in R and `pd.read_parquet` in Python.

### Data definitions

Details on selected data fields:

- `home_start_net`: Whether the home team starts the game with their net at `pos_x` (89,0) or `neg_x` (-89,0).
- `game_stint`: A stint is a stretch of play during which a set of distinct players is on the ice, and a new `game_stint` is
triggered whenever a player enters or leaves the ice.
- `sequence_id`: A sequence is a stretch of play uninterrupted by a whistle, and a new `sequence_id` is triggered for each faceoff-to-whistle stretch.
- `x`, `y`, `x_adj`, `y_adj`: `x` (from -100 to 100) and `y` (from -42.5 to 42.5) are the ice coordinates where the event occured, while `x_adj` and `y_adj` are adjusted so that the subject team's offensive zone is always to the right.
- `outcome`: Whether the event in question is successful, failed, or undetermined (game stoppage leading to neither a successful or failed outcome).
- `detail`: See bullet points under each `event_type` below for their corresponding details.

Description of the `event_type` fields:

- faceoff: success is credited to the team who first gains possession after the puck is dropped
    - recoveredwithentry: faceoff win with entry into the offensive zone
    - recoveredwithshotonnet: faceoff win with shot on net
    - recoveredwithexit: faceoff win with an exit out of the defensive zone
    - recoveredwithshotonnetandslotshot: faceoff win with slot shot on net
    - recoveredwithslotshot: faceoff win with slot shot attempt
    - recovered: faceoff win without any of the above
    - none: none of the above
- lpr: loose puck recovery
    - contested
    - faceoff / faceoffcontested
    - rebound / reboundcontested
    - opdump / opdumpcontested: lpr after an opposing team dumpin
    - nofore: no forechecker pressuring the lpr player
    - hipresopdump / hipresopdumpcontested: lpr with high pressure after an opposing team dumpin
    - none: none of the above
- shot
    - outside: unblocked non-slot shot attempt
    - outsideblocked: blocked non-slot shot attempt
    - slot: unblocked shot attempt from the home plate area
    - slotblocked: blocked shot attempt from the home plate area
- save
    - onfailedblock
    - none: none of the above
- pass
    - d2d / d2doffboards: defensive zone non-outlet passes
    - outlet / outletoffboards: defensive zone pass driving play forward
    - stretch / stretchoffboards: passes from the defensive zone received beyond the centre ice red line
    - ozentrystretch / ozentrystretchoffboards: offensive zone entry passes originating in the defensive zone
    - north / northoffboards: non-defensive zone passes driving play forward
    - south / southoffboards: non-defensive zone passes driving play backward
    - eastwest / eastwestoffboards: non-defensive zone passes driving play across the midline of the ice
    - ozentry / ozentryoffboards: offensive zone entry passes originating in the neutral zone
    - rush / rushoffboards: passes off the rush
    - slot: passes to the home plate area in the offensive zone
    - offboards: all other offboards passes
    - none: none of the above
- reception
    - ozentry: offensive zone entry pass reception
    - regular: all other pass reception attempts
- puckprotection: deke or body puck-protection attempt
- check: stick or body check attempt
- controlledexit: event for the team moving the puck via carry or pass from the defensive zone to the neutral zone
    - carry / pass
    - carrywithplay / passwithplay: controlled entry with successful play after
- block: pass or shot block attempt
- controlledentry: event for the team moving the puck via carry or pass from the defensive or neutral zone to the offensive zone
    - carrywithplaywithshotonnetandslotshot /
passwithplaywithshotonnetandslotshot: controlled entries with
successful play and slot shot on net
    - carrywithplaywithshotonnet / passwithplaywithshotonnet: controlled entries with successful play and shot on net
    - carrywithplaywithslotshot / passwithplaywithslotshot: controlled entries with successful play and slot shot attempt
    - carrywithshotonnetandslotshot / passwithshotonnetandslotshot: controlled entries with slot shot on net
    - carrywithshotonnet / passwithshotonnet: controlled entries with shot on net
    - carrywithslotshot / passwithslotshot: controlled entries with slot shot attempt
    - carrywithplay / passwithplay: all other carry/pass controlled entries with successful play
    - carry / pass: all other carry/pass controlled entries
- controlledentryagainst: event credited to the nearest defender when the opposing team moves the puck via carry or pass to the offensive zone
    - Number of attackers and defenders that are involved in the rush (1on0, 1on1, 1on2, 2on1, 2on2, 2on3, 3on1, 3on2, 3on3)
- carry: line carries for either blue line or the centre ice red line
- rebound
    - onfailedblock
    - none: none of the above
- dumpout
    - ice: icing
    - boards: off the boards
    - flip: flip out
- dumpin
    - dump
    - chip
- penaltydrawn: tripping, interference, hooking, holding, crosschecking, puckoverglass, boarding, roughing, delayofgame, highsticking, otherinfraction, slashing, elbowing, goalieinterference, charging, illegalchecktothehead
- penalty: tripping, interference, hooking, holding, crosschecking, puckoverglass, boarding, roughing, delayofgame, highsticking, otherinfraction, slashing, elbowing, goalieinterference, charging, illegalchecktothehead, toomanymen
- offside
- icing
- assist (first, second)
- soshot: shootout shot attempt
- sogoal: shootout goal

## Submission Requirements

- Deadline: February 19th, 2026
- Maximum 5 pages, exclusive of appendix
- Recommended maximum 25MB file size 

Submissions should focus on translating hockey data into clear, actionable intelligence for hockey decision-makers.

Each submission should include:

- A clear statement of impact, describing what hockey problem or decision the analysis aims to inform and why it matters.
- Key insights and conclusions, presented upfront and supported by evidence from the data.
- Actionable implications, outlining how the findings could influence coaching strategy, player usage, development, or front-office decision-making.
- Analytical approach, summarizing how Sportlogiq tracking and event data were used to generate the insights, including key assumptions and limitations.
- Clear structure and storytelling, ensuring the work is easy to follow and communicates effectively to a hockey operations audience.

Supporting code, models, or technical details may be included as an appendix and should reinforce the insights rather than serve as the primary focus of the submission.

Please submit either by emailing us at halohackathon@teamworks.com or creating a private Github repo, and please wait to share your results publicly until after the conference.

### Research Themes

Participants may explore any topic they choose. These three optional themes can be used as thought-starters.

**1. Player Evaluation**

Develop metrics or models to deepen our understanding of players' tendencies and abilities.

Examples:

- Develop an expected goals model incorporating the tracking data.
- Develop a player evaluation model for a specific event type, such as loose puck recovery performance based on positioning.

**2. Team Structure and Gameplay**

Assess elements of team play.

Examples:

- Assess team passing performance and/or strategy.
- Evaluate team patterns, such as formation detection at the time of a dump-in recovery.

**3. Pressure & Space Dynamics**

Quantify how players create or respond to pressure on the ice.

Examples:

- Examine defensive spacing and containment.
- Assess timing and positioning during transitions.
