## Make yourself comfortable: Using smartwatch-based Just-in-time Adaptive Interventions (JITAI) for mitigating urban-scale heat and noise

Public repository for the paper titled **'Make yourself comfortable: Using smartwatch-based Just-in-time Adaptive Interventions (JITAI) for mitigating urban-scale heat and noise'** using Cozie-Apple data collected during the `orenth` and `usk` experiments. In the paper, `orenth` is referred to as 'phase 1', and `usk` is referred to as 'phase 2'.

## Requirements

Install the required libraries:
```bash
python install -r requirements.txt
```

## Organization
The repository is organized in three main directories:
1. [1_data](./1_data/) <br>
   This directory contains the source data files.
2. [2_processing](./2_processing/)<br>
   This directory contains the notebooks for data processing and the output files of these notebooks.
3. [3_visualizations](./3_visualization/)<br>
   This directory contains all notebooks to create the data-based visualization of paper. The visualizations themselves are in a subdirectory named [img](./3_/_visualization/img).

## [Data](./1_data/)
The data in this repository consists of the data collected during the `orenth` and `usk` experiments.
The experiments lasted from October 2022 to July 2023.
There are four main source data files in the [./1_data/](1_data directory):
 - Onboarding survey answers: [1_onboarding_survey_all_participants.csv](./1_data/1_onboarding_survey_all_participants.csv)
 - Weekly survey answers: [2_weekly_survey_data_all_participants.csv](./1_data/2_weekly_survey_data_all_participants.csv)
 - JITAI messages sent in phase 1: [3_jitai_data_participants_phase1_orenth.csv](./1_data/3_jitai_data_participants_phase1_orenth.csv)
 - JITAI messages sent in phase 2: [4_to_be_published/1_data/4_jitai_data_participants_phase2_usk.csv](./1_data/4_jitai_data_participants_phase2_usk.csv)

### Column names JITAI data
| Field name         | Type     | Unit | Description |
|:-------------------|:---------|:-----|:------------|
| `action`           |  String  | -    | Action taken by AWS Lambda function for JITAI messages|
| `id_message`       |  String  | -    | ID of message sent or not sent to participant |
| `id_sender`        |  String  | -    | Sender of message |
| `recipients`       |  Integer | -    | Number of recipients of message |
| `response_code`    |  Integer | -    | HTTP response code of OneSignal push notification API |
| `timestamp_lambda` |  String  | -    | Timestamp of when AWS Lambda function for JITAI messages is executed |

#### Values for `action` column
| Values in `action` column                                                          | Description |
|:-----------------------------------------------------------------------------------|:------------|
| no notification, conditions for message id not met.                                | No push notification is sent as none of the conditions are met. |
| send notification                                                                  | Push notification is sent. |
| no action, duration threshold not reached. (MIN_JITAI_MESSAGE_INTERVAL)            | Conditions to send a push notification are met, but no push notification is sent as there was already a push notification sent shortly before this one to the same participant. |
| no action, duration threshold not reached for message id ('send_interval_min').    | Conditions to send a push notification are met, but no push notification is sent as there was already a push notification sent with the same `id_message` shortly before. |
| no action, max message threshold ('send_count_max' for message id) reached.        | Conditions to send a push notification are met, but no push notification is sent as the daily limit for the number of push notifications with the same `id_message` was reached. |
| no action, max message threshold (all jitai) reached. (MAX_JITAI_MESSAGES_PER_DAY) | Conditions to send a push notification are met, but no push notification is sent as the daily limit for the number of push notifications over all was reached.|
| virtually fire notification                                                        | A push notification would have been sent if the debug mode had been disabled. |

#### Values for `id_message` column
| Values `id_message` column | Description |
|:---------------------------|:------------|
| half_way                   | Inform participant that they have completed 50% of the experiment. |
| jitai_noise                | JITAI message using a noise threshold value. |
| jitai_temperature          | JITAI message using an outdoor air temperature threshold. |
| jitai_temperature_adaptive | JITAI message using personalized temperature threshold. |
| jitai_noise_adaptive       | JITAI message using personalized noise threshold. |

#### Values for `recipients` column
| Values `recipients` column | Description |
|:---------------------------|:------------|
| -1                         | No recipients of a push notification because no message was sent. |
| 0                          | No recipients of a push notification despite the attempt to send a push notification. |
| 1                          | One recipient of a push notification. |

## [Processing](./2_processing/)
The personality scores are computed in [1_personality_score_calculation.ipynb](/2_processing/1_personality_score_calculation.ipynb). The scores are then saved in [2_all_personality_scores.csv](4_to_be_published\2_processing\2_all_personality_scores.csv). The scores are also merged with the weekly survey responses and saved in [3_weekly_survey_with_personality.csv](4_to_be_published\2_processing\3_weekly_survey_with_personality.csv).

Further, there are two notebooks for minor data processing:
   - [Labelling missing responses](./2_processing/2_weekly_survey_data_no_responses.ipynb) (Output file: [5_weekly_survey_with_personality_no_nan.csv](./2_processing/5_weekly_survey_with_personality_no_nan.csv))
   - [Removing redundant respones](./2_processing/6_weekly_survey_data_only_one_response_per_week.ipynb) (Output file: [7_weekly_survey_with_personality_no_nan_one_response_per_week.csv](./2_processing/7_weekly_survey_with_personality_no_nan_one_response_per_week.csv))

## [Visualizations](./3_visualizations/)
The notebooks to create the data-based figures can be found in the [visualizations](./3_visualizations/) directory. The resulting images can be found in the [img](./3_visualizations/img/) sub-directory.
| Figure | Notebook |
|---|---|
| [Figure 7](./3_visualizations/img/figure_7.pdf) | [figure_7_8_10.ipynb](./3_visualizations/figure_7_8_10.ipynb) |
| [Figure 8](./3_visualizations/img/figure_8.pdf) | [figure_7_8_10.ipynb](./3_visualizations/figure_7_8_10.ipynb) |
| [Figure 9](./3_visualizations/img/figure_9.png) | [figure_9.ipynb](./3_visualizations/figure_7_8_10.ipynb) |
| [Figure 10](./3_visualizations/img/figure_10.pdf) | [figure_7_8_10.ipynb](./3_visualizations/figure_7_8_10.ipynb) |
| [Figure 12a](./3_visualizations/img/figure_12_orenth_before_after_jitai.png) | [figure_12_jitai_changes.ipynb](./3_visualizations/figure_12_jitai_changes.ipynb) |
| [Figure 12b](./3_visualizations/img/usk_before_after_jitai.png) | [figure_12_jitai_changes.ipynb](./3_visualizations/figure_12_jitai_changes.ipynb) |