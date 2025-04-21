# openpsychometrics__ambi_dec_2019

## Table of contents

## Overview

This Power BI dashboard visualizes data collected using the Analog to Multiple Broadband Inventories (AMBI), a psychological assessment tool designed to model personality traits across multiple established frameworks.

- **Data Source**: [OpenPsychometrics - AMBI Test Dataset](https://openpsychometrics.org/_rawdata/AMBI_data_Dec2019.zip)

- **Collected**: November 2, 2019

- **Sample Size**: 2,017 participants

- **Items**: 181 Likert-scale items per participant

- **Link to the AMBI test**: [Analog to Multiple Broadband Inventories](https://openpsychometrics.org/tests/AMBI/)

## Objective

To create an interactive dashboard that provides insights into patterns and distributions in personality data, enabling exploration of:

- Factor structure and item loadings

- Score distributions across traits

- Demographic splits (if available)

- Correlations and comparative analysis across scales

## Dataset Description

- **Format**: CSV

- **Responses**: 1–7 Likert scale per item

- **Features**:

  - 181 personality items

  - No personal identifiable data

  - Pre-cleaned (assumed from source)

## Dashboard Features

- Factor Breakdown: Visualization of trait clusters

- Item Analysis: Response trends across all items

- Heatmaps: Inter-item correlation

- Dimensionality Reduction: PCA/t-SNE for pattern recognition

- Custom Filters: Score thresholds, clusters, etc.

## Setup Instructions

1. Download the dataset from the [source link](https://openpsychometrics.org/_rawdata/AMBI_data_Dec2019.zip)

2. Open Power BI Desktop

3. Load the dataset (.csv)

4. Apply the provided Power Query transformations (optional)

5. Load and explore the dashboard

### Load the `question_map.json`

1. Get data > Select JSON file type > Navigate to the root directory openpsychometrics__ambi_dec_2019 > Select the json file from file path `data/question_map.json`

2. In the powerbi query editor, select the question_map source. There should be a table automatically created by Powerbi.

3. Navigate to the `Transform` tab.

4. In the `Table` section, click the dropdown for the `Use First Row as Headers` and select `Use Headers as First Row`.

This will add the ff entries to the `Applied Steps` pane:

- `Demoted Headers`
- `Changed Type 1`

You will notice the headers are replaced with a sequence with format `Column<n>`.

The previous headers `Q1, Q2, Q3, ..., Q<n>, Q181` are now the first row of the table.

The actual question texts which were written on the first row are now in the second row.

5. In the `Transform` tab, click the `Transpose` button to transpose the table.

This will take both rows as the columns. The new headers will now be `Column1, Column2`.

6. Rename the headers to `qid, question`.

7. Navigate to the `Home` tab and select `Close & Apply`. Wait while Power BI loads the new data source.

### Unpivot q columns of raw data

1. For the sake of tracing back my steps, I'm duplicating the raw ambi table. To do this, follow the steps below:

- 1.1 Go to Home → Transform Data (opens Power Query)

- 1.2 In the left panel, right-click on raw_ambi_data

- 1.3 Select Duplicate

- 1.4 Rename it (e.g., clean_ambi_data)

- 1.5 Close & Apply

2. Since the table currently has a wide format, I have to unpivot it.

Currently we have this wide format:
User    | Q1A | Q1I | Q1E  | Q2A | Q2I | Q2E
1       | 5   | 136 | 1506 | 6   | 71  | 3542
2       | 6   | 118 | 3539 | 2   | 117 | 2282

After Unpivotting, it should look like this:
User    | Question | Answer | Index | Elapsed
1       | 1	   | 5      | 136   | 1506
1       | 2	   | 6      | 71    | 3542
2       | 1	   | 6      | 118   | 3539
2       | 2	   | 2      | 117   | 2282

- 2.1 Go to Power Query Editor (Home → Transform Data)

- 2.2 Select table clean_ambi_data

- 2.3 Click Advanced Editor (under Home tab)

- 2.4 Replace the code with the m code written in `clean_ambi_data_advanced_m_code.txt`.

## Limitations

- No demographic data (age, gender, etc.)

- Self-reported data, subject to response biases

- No external validity checks

## License

The dataset is available under the terms described by OpenPsychometrics. This dashboard is for educational and research purposes only.

