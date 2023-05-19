# WCSF_SolutionRoom
Repository connected to the Solution Room of the WSCF 2023 in Groningen. Contains a prepocessed data sample of football (soccer) player tracking data, plus some basic variables derived from the sample

## Data Guide
The dataset folder in this repo contains pre-processing position tracking data and derived aggregated data from a single match. The dataset has been collected a couple of seasons ago in a match player in a professional european football league, and has been made anonymous for research purposes. The following files are provided for a match:
- `positions.csv`
- `physical_kpis_home.csv`
- `physical_kpis_away.csv`
- `centroid_positions.csv`

Please find a detailed explanation per file below. 

### Positions File
Position data has originally been tracked using an optical tracking system in 25 Hz, and contains xy positions for all players and the ball. 

During preprocessing, data is downsampled to 10 Hz, and smoothed. The data is "mirrored" so that for any player, the own goal is always on -52.5 and the other goal is always at +52.5. As a result you will have to "de-mirror" the data if you want to make realistic visualizations or if you want to compute absolute distances between players from two separate teams. Furthermore, ball coordinates are added as columns to the player rows, the team game phase is determined per frame, and speed and acceleration values are derived from the tracking data. 

The resulting dataframe contains a row per player per timestamp. 

The datamodel is as follows:
- `x` >>> x coordinate of tracked object, note that x-axis runs from -55m to +55m including out of bounds areas and field length is 105.0 meter (on field area runs from -52.5 to +52.5). _the axis runs along the center of the field from goal to goal_
- `y` >>> y coordinate of tracked object, note that x-axis runs from -35m to +35m including out of bounds areas and field width is 68.0 meter (on field area runs from -34 to +34). _the axis runs along the midline of the field from sideline to sideline_.
- `speed` >>> speed in m/s, computed based on the mean value in moving window of 10 frames (1000 ms). 
- `acceleration` >>> acceleration in m/s^2, derived from the speed value.
- `group_id` >>> 1 for the home team, 2 for the away team. 
- `timestamp_ms` >>> timestamp of the frame in milliseconds since kick-off.
- `AliveFlag` >>> represents if the ball is alive ("in-play") or dead. 1 = Alive / 0 = Dead.
- `PossessionFlag` >>> represents the group id of the team in possession on that timeframe.
- `x_ball` >>> x coordinate of the ball, relative to the player to which the row belongs.
- `y_ball` >>> y coordinate of the ball, relative to the player to which the row belongs.
- `team_game_phase` >>> Represents the game-phase a team is in for a given frame, can hold the following values: 
- `player_id` >>> unique player identifier. Note that id's for this research set have been randomly generated and do not correspond to any existing id in source databases. 

### Physical KPIS
For both teams in the match, aggregated physical data is provided per player. The physical kpis entail the total ditance covered and the average distance covered per minute, both for the full match as well as per 5 minute part. Furthermore, it contains distances covered regardless of speed zones (all frames with a speed of 0-12.5 m.s / 0-36 km.h are included), as well as the distance covered in 5 specific speed zones. Additionally, the KPI's are split between Gross (all frames included) as well as Net (only frames where `AliveFlag == 1` are included), as well as net average distance per min for attacking and defending frames. Finaly, the run count in 3 different categories is provided. 

The files should be pretty much self-explanatory. Please note that the player names are replaced by the randomly generated player id's that also be found in the tracking data. 

### Centroid Positions
...
