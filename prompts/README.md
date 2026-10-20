# Prompts

Every system prompt used in the paper, byte-identical to the text sent to the models.

## Call protocol

- One call per (image, embodiment) pair: the system prompt, then a user message with the benchmark image (original JPEG bytes) followed by [`user_message.txt`](user_message.txt), the user message of the NaviTrace benchmark, left unchanged.
- Temperature 1.0, low reasoning effort, output limit of 5,000 tokens, no structured-output mode.
- The deterministic repair is applied to every output, for both methods, before scoring and before execution. It clamps each waypoint to [0, 1]² and prepends the start point [0.5, 0.95] when the first waypoint lies farther than 0.03 from it.

## Main prompts

| File | Method |
|---|---|
| [`prism_nav.txt`](prism_nav.txt) | PRISM-Nav. The decision schema (4–6 waypoints, goal fields before the trajectory) and the six ordered stages |
| [`navitrace_baseline.txt`](navitrace_baseline.txt) | NaviTrace, the zero-shot prompt of the benchmark (used as the baseline) |

## Ablations (Table IV and Section IV-D)

Each row of Table IV changes one element of the row above.

| File | Row or experiment | Change |
|---|---|---|
| [`navitrace_baseline.txt`](navitrace_baseline.txt) | NaviTrace | |
| [`ablations/1_waypoint_bound.txt`](ablations/1_waypoint_bound.txt) | + waypoint bound | adds "Use 4-6 waypoints" |
| [`ablations/2_goal_after_path.txt`](ablations/2_goal_after_path.txt) | + goal after path | JSON output with the goal fields written after the trajectory |
| [`ablations/3_goal_before_path_schema.txt`](ablations/3_goal_before_path_schema.txt) | + goal before path (schema) | the same fields with the goals written before the trajectory |
| [`ablations/4_unordered_stages.txt`](ablations/4_unordered_stages.txt) | + unordered stages | the content of the six stages as one paragraph that may be weighed in any order and revised |
| [`prism_nav.txt`](prism_nav.txt) | + stage order (PRISM-Nav) | the same stages, numbered, with no stage allowed to revise an earlier one |
| [`oracle_goal_line.txt`](oracle_goal_line.txt) | Oracle goal | PRISM-Nav with this line appended to the task line, carrying the endpoint of the reference trace |
| [`ablations/ordered_stages_goal_after_path.txt`](ablations/ordered_stages_goal_after_path.txt) | Section IV-D | PRISM-Nav with the trajectory written before the goal fields |
| [`ablations/waypoint_bound_plan_and_solve.txt`](ablations/waypoint_bound_plan_and_solve.txt) | Section IV-D | the waypoint-bound prompt with a plan-and-solve instruction |
| [`ablations/revisable_goal_bound_6_8.txt`](ablations/revisable_goal_bound_6_8.txt) | Section IV-D | ordered stages whose goal a later stage may revise, at the earlier bound of 6–8 waypoints |
| [`ablations/navitrace_rules_restored.txt`](ablations/navitrace_rules_restored.txt) | Section IV-D | PRISM-Nav with the occlusion and traffic rules of the NaviTrace prompt restored |
| [`ablations/no_executed_goal.txt`](ablations/no_executed_goal.txt) | Section IV-A | PRISM-Nav without the executed-goal field |

## Waypoint bound (Table V)

PRISM-Nav with only the bound changed: [`none`](bound_sweep/none.txt), [`2–4`](bound_sweep/2_4.txt), [`3–5`](bound_sweep/3_5.txt), [`4–6`](bound_sweep/4_6.txt) (identical to `prism_nav.txt`), [`6–8`](bound_sweep/6_8.txt), [`6–12`](bound_sweep/6_12.txt), [`6–16`](bound_sweep/6_16.txt).
