# Battery Swap Routing Optimizer

## Assumptions

- **Riders**: 100 riders with random coordinates in Pune (lat: 18.48–18.52, lng: 73.88–73.92), SOC 20–35%, 50% idle, 50% on_gig.
- **Stations**: 3 stations with initial queue lengths 0–5.
- **Speed**: 30 km/h for travel time calculations.
- **SOC Consumption**: 4% per km.
- **Swap Time**: 4 minutes, restoring SOC to 100%.
- **Return Location**: Idle riders return to their starting point; on_gig riders return to a slightly offset location.
- **Time Window**: 60 minutes from Current Time.
- **SOC Threshold**: Riders need a swap if projected SOC after travel is 10–20% or current SOC ≤ 10%.

## Algorithm

A greedy heuristic is used:

1. **Prioritize Riders**: Sort by SOC (lowest first) for urgency.
2. **Check Swap Need**: Calculate SOC after travel (gig + station for on_gig, station only for idle). Flag riders for a swap if projected SOC is 10–20% or current SOC ≤ 10%.
3. **Indicate Low SOC**: Log riders needing swaps, showing rider_id, projected SOC, and nearest station.
4. **Assign Stations**: Select the nearest station minimizing detour distance (to station + back to return location), ensuring queue ≤ 5, arrival SOC ≥ 10%, and swap within 60 minutes.
5. **Timestamps**: Compute depart, arrive, swap start/end times, and return coordinates.
6. **Queue Management**: Maintain a timeline per station to track queue length.

## Objective Fulfillment

- **SOC ≥ 10%**: Ensured by checking SOC after travel and scheduling swaps if projected SOC is 10–20% or current SOC ≤ 10%. Riders at risk are logged.
- **Minimize Detours**: Greedy selection of nearest station; total detour printed.
- **Queue ≤ 5**: Timeline ensures no more than 5 riders at any station at any time.

## Scalability

- **Current**: O(n \* m) complexity (n = 100 riders, m = 3 stations). Suitable for small-scale problems.

## Outputs

- **Input**: `riders.json`, `stations.json`.
- **Output**: `plan_output.json` with rider_id, station_id, depart_ts, arrive_ts, swap_start_ts, swap_end_ts, eta_back_lat, eta_back_lng.
- **Visualization**: Map Visualization (`map_visualization.html`) showing map of pune.
- **Indication**: Console logs riders needing swaps and unassigned riders with reasons.
