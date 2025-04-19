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
user_id | Q1A | Q1I | Q1E  | Q2A | Q2I | Q2E
1       | 5   | 136 | 1506 | 6   | 71  | 3542
2       | 6   | 118 | 3539 | 2   | 117 | 2282

After Unpivotting, it should look like this:
user_id | question_id | answer | index | elapsed
1       | 1	      | 5      | 136   | 1506
1       | 2	      | 6      | 71    | 3542
2       | 1	      | 6      | 118   | 3539
2       | 2	      | 2      | 117   | 2282

- 2.1 Go to Power Query Editor (Home → Transform Data)

- 2.2 Select table clean_ambi_data

- 2.3 Click Advanced Editor (under Home tab)

- 2.4 (WIP) Replace the code with this m code:

```m
let
    Source = Csv.Document(File.Contents("Z:\powerbi\powerbi\openpsychometrics__ambi_dec_2019\data\data.csv"),[Delimiter="	", Columns=543, Encoding=1252, QuoteStyle=QuoteStyle.None]),

    #"Promoted Headers" = Table.PromoteHeaders(Source, [PromoteAllScalars=true]),

    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"Q1A", Int64.Type}, {"Q1I", Int64.Type}, {"Q1E", Int64.Type}, {"Q2A", Int64.Type}, {"Q2I", Int64.Type}, {"Q2E", Int64.Type}, {"Q3A", Int64.Type}, {"Q3I", Int64.Type}, {"Q3E", Int64.Type}, {"Q4A", Int64.Type}, {"Q4I", Int64.Type}, {"Q4E", Int64.Type}, {"Q5A", Int64.Type}, {"Q5I", Int64.Type}, {"Q5E", Int64.Type}, {"Q6A", Int64.Type}, {"Q6I", Int64.Type}, {"Q6E", Int64.Type}, {"Q7A", Int64.Type}, {"Q7I", Int64.Type}, {"Q7E", Int64.Type}, {"Q8A", Int64.Type}, {"Q8I", Int64.Type}, {"Q8E", Int64.Type}, {"Q9A", Int64.Type}, {"Q9I", Int64.Type}, {"Q9E", Int64.Type}, {"Q10A", Int64.Type}, {"Q10I", Int64.Type}, {"Q10E", Int64.Type}, {"Q11A", Int64.Type}, {"Q11I", Int64.Type}, {"Q11E", Int64.Type}, {"Q12A", Int64.Type}, {"Q12I", Int64.Type}, {"Q12E", Int64.Type}, {"Q13A", Int64.Type}, {"Q13I", Int64.Type}, {"Q13E", Int64.Type}, {"Q14A", Int64.Type}, {"Q14I", Int64.Type}, {"Q14E", Int64.Type}, {"Q15A", Int64.Type}, {"Q15I", Int64.Type}, {"Q15E", Int64.Type}, {"Q16A", Int64.Type}, {"Q16I", Int64.Type}, {"Q16E", Int64.Type}, {"Q17A", Int64.Type}, {"Q17I", Int64.Type}, {"Q17E", Int64.Type}, {"Q18A", Int64.Type}, {"Q18I", Int64.Type}, {"Q18E", Int64.Type}, {"Q19A", Int64.Type}, {"Q19I", Int64.Type}, {"Q19E", Int64.Type}, {"Q20A", Int64.Type}, {"Q20I", Int64.Type}, {"Q20E", Int64.Type}, {"Q21A", Int64.Type}, {"Q21I", Int64.Type}, {"Q21E", Int64.Type}, {"Q22A", Int64.Type}, {"Q22I", Int64.Type}, {"Q22E", Int64.Type}, {"Q23A", Int64.Type}, {"Q23I", Int64.Type}, {"Q23E", Int64.Type}, {"Q24A", Int64.Type}, {"Q24I", Int64.Type}, {"Q24E", Int64.Type}, {"Q25A", Int64.Type}, {"Q25I", Int64.Type}, {"Q25E", Int64.Type}, {"Q26A", Int64.Type}, {"Q26I", Int64.Type}, {"Q26E", Int64.Type}, {"Q27A", Int64.Type}, {"Q27I", Int64.Type}, {"Q27E", Int64.Type}, {"Q28A", Int64.Type}, {"Q28I", Int64.Type}, {"Q28E", Int64.Type}, {"Q29A", Int64.Type}, {"Q29I", Int64.Type}, {"Q29E", Int64.Type}, {"Q30A", Int64.Type}, {"Q30I", Int64.Type}, {"Q30E", Int64.Type}, {"Q31A", Int64.Type}, {"Q31I", Int64.Type}, {"Q31E", Int64.Type}, {"Q32A", Int64.Type}, {"Q32I", Int64.Type}, {"Q32E", Int64.Type}, {"Q33A", Int64.Type}, {"Q33I", Int64.Type}, {"Q33E", Int64.Type}, {"Q34A", Int64.Type}, {"Q34I", Int64.Type}, {"Q34E", Int64.Type}, {"Q35A", Int64.Type}, {"Q35I", Int64.Type}, {"Q35E", Int64.Type}, {"Q36A", Int64.Type}, {"Q36I", Int64.Type}, {"Q36E", Int64.Type}, {"Q37A", Int64.Type}, {"Q37I", Int64.Type}, {"Q37E", Int64.Type}, {"Q38A", Int64.Type}, {"Q38I", Int64.Type}, {"Q38E", Int64.Type}, {"Q39A", Int64.Type}, {"Q39I", Int64.Type}, {"Q39E", Int64.Type}, {"Q40A", Int64.Type}, {"Q40I", Int64.Type}, {"Q40E", Int64.Type}, {"Q41A", Int64.Type}, {"Q41I", Int64.Type}, {"Q41E", Int64.Type}, {"Q42A", Int64.Type}, {"Q42I", Int64.Type}, {"Q42E", Int64.Type}, {"Q43A", Int64.Type}, {"Q43I", Int64.Type}, {"Q43E", Int64.Type}, {"Q44A", Int64.Type}, {"Q44I", Int64.Type}, {"Q44E", Int64.Type}, {"Q45A", Int64.Type}, {"Q45I", Int64.Type}, {"Q45E", Int64.Type}, {"Q46A", Int64.Type}, {"Q46I", Int64.Type}, {"Q46E", Int64.Type}, {"Q47A", Int64.Type}, {"Q47I", Int64.Type}, {"Q47E", Int64.Type}, {"Q48A", Int64.Type}, {"Q48I", Int64.Type}, {"Q48E", Int64.Type}, {"Q49A", Int64.Type}, {"Q49I", Int64.Type}, {"Q49E", Int64.Type}, {"Q50A", Int64.Type}, {"Q50I", Int64.Type}, {"Q50E", Int64.Type}, {"Q51A", Int64.Type}, {"Q51I", Int64.Type}, {"Q51E", Int64.Type}, {"Q52A", Int64.Type}, {"Q52I", Int64.Type}, {"Q52E", Int64.Type}, {"Q53A", Int64.Type}, {"Q53I", Int64.Type}, {"Q53E", Int64.Type}, {"Q54A", Int64.Type}, {"Q54I", Int64.Type}, {"Q54E", Int64.Type}, {"Q55A", Int64.Type}, {"Q55I", Int64.Type}, {"Q55E", Int64.Type}, {"Q56A", Int64.Type}, {"Q56I", Int64.Type}, {"Q56E", Int64.Type}, {"Q57A", Int64.Type}, {"Q57I", Int64.Type}, {"Q57E", Int64.Type}, {"Q58A", Int64.Type}, {"Q58I", Int64.Type}, {"Q58E", Int64.Type}, {"Q59A", Int64.Type}, {"Q59I", Int64.Type}, {"Q59E", Int64.Type}, {"Q60A", Int64.Type}, {"Q60I", Int64.Type}, {"Q60E", Int64.Type}, {"Q61A", Int64.Type}, {"Q61I", Int64.Type}, {"Q61E", Int64.Type}, {"Q62A", Int64.Type}, {"Q62I", Int64.Type}, {"Q62E", Int64.Type}, {"Q63A", Int64.Type}, {"Q63I", Int64.Type}, {"Q63E", Int64.Type}, {"Q64A", Int64.Type}, {"Q64I", Int64.Type}, {"Q64E", Int64.Type}, {"Q65A", Int64.Type}, {"Q65I", Int64.Type}, {"Q65E", Int64.Type}, {"Q66A", Int64.Type}, {"Q66I", Int64.Type}, {"Q66E", Int64.Type}, {"Q67A", Int64.Type}, {"Q67I", Int64.Type}, {"Q67E", Int64.Type}, {"Q68A", Int64.Type}, {"Q68I", Int64.Type}, {"Q68E", Int64.Type}, {"Q69A", Int64.Type}, {"Q69I", Int64.Type}, {"Q69E", Int64.Type}, {"Q70A", Int64.Type}, {"Q70I", Int64.Type}, {"Q70E", Int64.Type}, {"Q71A", Int64.Type}, {"Q71I", Int64.Type}, {"Q71E", Int64.Type}, {"Q72A", Int64.Type}, {"Q72I", Int64.Type}, {"Q72E", Int64.Type}, {"Q73A", Int64.Type}, {"Q73I", Int64.Type}, {"Q73E", Int64.Type}, {"Q74A", Int64.Type}, {"Q74I", Int64.Type}, {"Q74E", Int64.Type}, {"Q75A", Int64.Type}, {"Q75I", Int64.Type}, {"Q75E", Int64.Type}, {"Q76A", Int64.Type}, {"Q76I", Int64.Type}, {"Q76E", Int64.Type}, {"Q77A", Int64.Type}, {"Q77I", Int64.Type}, {"Q77E", Int64.Type}, {"Q78A", Int64.Type}, {"Q78I", Int64.Type}, {"Q78E", Int64.Type}, {"Q79A", Int64.Type}, {"Q79I", Int64.Type}, {"Q79E", Int64.Type}, {"Q80A", Int64.Type}, {"Q80I", Int64.Type}, {"Q80E", Int64.Type}, {"Q81A", Int64.Type}, {"Q81I", Int64.Type}, {"Q81E", Int64.Type}, {"Q82A", Int64.Type}, {"Q82I", Int64.Type}, {"Q82E", Int64.Type}, {"Q83A", Int64.Type}, {"Q83I", Int64.Type}, {"Q83E", Int64.Type}, {"Q84A", Int64.Type}, {"Q84I", Int64.Type}, {"Q84E", Int64.Type}, {"Q85A", Int64.Type}, {"Q85I", Int64.Type}, {"Q85E", Int64.Type}, {"Q86A", Int64.Type}, {"Q86I", Int64.Type}, {"Q86E", Int64.Type}, {"Q87A", Int64.Type}, {"Q87I", Int64.Type}, {"Q87E", Int64.Type}, {"Q88A", Int64.Type}, {"Q88I", Int64.Type}, {"Q88E", Int64.Type}, {"Q89A", Int64.Type}, {"Q89I", Int64.Type}, {"Q89E", Int64.Type}, {"Q90A", Int64.Type}, {"Q90I", Int64.Type}, {"Q90E", Int64.Type}, {"Q91A", Int64.Type}, {"Q91I", Int64.Type}, {"Q91E", Int64.Type}, {"Q92A", Int64.Type}, {"Q92I", Int64.Type}, {"Q92E", Int64.Type}, {"Q93A", Int64.Type}, {"Q93I", Int64.Type}, {"Q93E", Int64.Type}, {"Q94A", Int64.Type}, {"Q94I", Int64.Type}, {"Q94E", Int64.Type}, {"Q95A", Int64.Type}, {"Q95I", Int64.Type}, {"Q95E", Int64.Type}, {"Q96A", Int64.Type}, {"Q96I", Int64.Type}, {"Q96E", Int64.Type}, {"Q97A", Int64.Type}, {"Q97I", Int64.Type}, {"Q97E", Int64.Type}, {"Q98A", Int64.Type}, {"Q98I", Int64.Type}, {"Q98E", Int64.Type}, {"Q99A", Int64.Type}, {"Q99I", Int64.Type}, {"Q99E", Int64.Type}, {"Q100A", Int64.Type}, {"Q100I", Int64.Type}, {"Q100E", Int64.Type}, {"Q101A", Int64.Type}, {"Q101I", Int64.Type}, {"Q101E", Int64.Type}, {"Q102A", Int64.Type}, {"Q102I", Int64.Type}, {"Q102E", Int64.Type}, {"Q103A", Int64.Type}, {"Q103I", Int64.Type}, {"Q103E", Int64.Type}, {"Q104A", Int64.Type}, {"Q104I", Int64.Type}, {"Q104E", Int64.Type}, {"Q105A", Int64.Type}, {"Q105I", Int64.Type}, {"Q105E", Int64.Type}, {"Q106A", Int64.Type}, {"Q106I", Int64.Type}, {"Q106E", Int64.Type}, {"Q107A", Int64.Type}, {"Q107I", Int64.Type}, {"Q107E", Int64.Type}, {"Q108A", Int64.Type}, {"Q108I", Int64.Type}, {"Q108E", Int64.Type}, {"Q109A", Int64.Type}, {"Q109I", Int64.Type}, {"Q109E", Int64.Type}, {"Q110A", Int64.Type}, {"Q110I", Int64.Type}, {"Q110E", Int64.Type}, {"Q111A", Int64.Type}, {"Q111I", Int64.Type}, {"Q111E", Int64.Type}, {"Q112A", Int64.Type}, {"Q112I", Int64.Type}, {"Q112E", Int64.Type}, {"Q113A", Int64.Type}, {"Q113I", Int64.Type}, {"Q113E", Int64.Type}, {"Q114A", Int64.Type}, {"Q114I", Int64.Type}, {"Q114E", Int64.Type}, {"Q115A", Int64.Type}, {"Q115I", Int64.Type}, {"Q115E", Int64.Type}, {"Q116A", Int64.Type}, {"Q116I", Int64.Type}, {"Q116E", Int64.Type}, {"Q117A", Int64.Type}, {"Q117I", Int64.Type}, {"Q117E", Int64.Type}, {"Q118A", Int64.Type}, {"Q118I", Int64.Type}, {"Q118E", Int64.Type}, {"Q119A", Int64.Type}, {"Q119I", Int64.Type}, {"Q119E", Int64.Type}, {"Q120A", Int64.Type}, {"Q120I", Int64.Type}, {"Q120E", Int64.Type}, {"Q121A", Int64.Type}, {"Q121I", Int64.Type}, {"Q121E", Int64.Type}, {"Q122A", Int64.Type}, {"Q122I", Int64.Type}, {"Q122E", Int64.Type}, {"Q123A", Int64.Type}, {"Q123I", Int64.Type}, {"Q123E", Int64.Type}, {"Q124A", Int64.Type}, {"Q124I", Int64.Type}, {"Q124E", Int64.Type}, {"Q125A", Int64.Type}, {"Q125I", Int64.Type}, {"Q125E", Int64.Type}, {"Q126A", Int64.Type}, {"Q126I", Int64.Type}, {"Q126E", Int64.Type}, {"Q127A", Int64.Type}, {"Q127I", Int64.Type}, {"Q127E", Int64.Type}, {"Q128A", Int64.Type}, {"Q128I", Int64.Type}, {"Q128E", Int64.Type}, {"Q129A", Int64.Type}, {"Q129I", Int64.Type}, {"Q129E", Int64.Type}, {"Q130A", Int64.Type}, {"Q130I", Int64.Type}, {"Q130E", Int64.Type}, {"Q131A", Int64.Type}, {"Q131I", Int64.Type}, {"Q131E", Int64.Type}, {"Q132A", Int64.Type}, {"Q132I", Int64.Type}, {"Q132E", Int64.Type}, {"Q133A", Int64.Type}, {"Q133I", Int64.Type}, {"Q133E", Int64.Type}, {"Q134A", Int64.Type}, {"Q134I", Int64.Type}, {"Q134E", Int64.Type}, {"Q135A", Int64.Type}, {"Q135I", Int64.Type}, {"Q135E", Int64.Type}, {"Q136A", Int64.Type}, {"Q136I", Int64.Type}, {"Q136E", Int64.Type}, {"Q137A", Int64.Type}, {"Q137I", Int64.Type}, {"Q137E", Int64.Type}, {"Q138A", Int64.Type}, {"Q138I", Int64.Type}, {"Q138E", Int64.Type}, {"Q139A", Int64.Type}, {"Q139I", Int64.Type}, {"Q139E", Int64.Type}, {"Q140A", Int64.Type}, {"Q140I", Int64.Type}, {"Q140E", Int64.Type}, {"Q141A", Int64.Type}, {"Q141I", Int64.Type}, {"Q141E", Int64.Type}, {"Q142A", Int64.Type}, {"Q142I", Int64.Type}, {"Q142E", Int64.Type}, {"Q143A", Int64.Type}, {"Q143I", Int64.Type}, {"Q143E", Int64.Type}, {"Q144A", Int64.Type}, {"Q144I", Int64.Type}, {"Q144E", Int64.Type}, {"Q145A", Int64.Type}, {"Q145I", Int64.Type}, {"Q145E", Int64.Type}, {"Q146A", Int64.Type}, {"Q146I", Int64.Type}, {"Q146E", Int64.Type}, {"Q147A", Int64.Type}, {"Q147I", Int64.Type}, {"Q147E", Int64.Type}, {"Q148A", Int64.Type}, {"Q148I", Int64.Type}, {"Q148E", Int64.Type}, {"Q149A", Int64.Type}, {"Q149I", Int64.Type}, {"Q149E", Int64.Type}, {"Q150A", Int64.Type}, {"Q150I", Int64.Type}, {"Q150E", Int64.Type}, {"Q151A", Int64.Type}, {"Q151I", Int64.Type}, {"Q151E", Int64.Type}, {"Q152A", Int64.Type}, {"Q152I", Int64.Type}, {"Q152E", Int64.Type}, {"Q153A", Int64.Type}, {"Q153I", Int64.Type}, {"Q153E", Int64.Type}, {"Q154A", Int64.Type}, {"Q154I", Int64.Type}, {"Q154E", Int64.Type}, {"Q155A", Int64.Type}, {"Q155I", Int64.Type}, {"Q155E", Int64.Type}, {"Q156A", Int64.Type}, {"Q156I", Int64.Type}, {"Q156E", Int64.Type}, {"Q157A", Int64.Type}, {"Q157I", Int64.Type}, {"Q157E", Int64.Type}, {"Q158A", Int64.Type}, {"Q158I", Int64.Type}, {"Q158E", Int64.Type}, {"Q159A", Int64.Type}, {"Q159I", Int64.Type}, {"Q159E", Int64.Type}, {"Q160A", Int64.Type}, {"Q160I", Int64.Type}, {"Q160E", Int64.Type}, {"Q161A", Int64.Type}, {"Q161I", Int64.Type}, {"Q161E", Int64.Type}, {"Q162A", Int64.Type}, {"Q162I", Int64.Type}, {"Q162E", Int64.Type}, {"Q163A", Int64.Type}, {"Q163I", Int64.Type}, {"Q163E", Int64.Type}, {"Q164A", Int64.Type}, {"Q164I", Int64.Type}, {"Q164E", Int64.Type}, {"Q165A", Int64.Type}, {"Q165I", Int64.Type}, {"Q165E", Int64.Type}, {"Q166A", Int64.Type}, {"Q166I", Int64.Type}, {"Q166E", Int64.Type}, {"Q167A", Int64.Type}, {"Q167I", Int64.Type}, {"Q167E", Int64.Type}, {"Q168A", Int64.Type}, {"Q168I", Int64.Type}, {"Q168E", Int64.Type}, {"Q169A", Int64.Type}, {"Q169I", Int64.Type}, {"Q169E", Int64.Type}, {"Q170A", Int64.Type}, {"Q170I", Int64.Type}, {"Q170E", Int64.Type}, {"Q171A", Int64.Type}, {"Q171I", Int64.Type}, {"Q171E", Int64.Type}, {"Q172A", Int64.Type}, {"Q172I", Int64.Type}, {"Q172E", Int64.Type}, {"Q173A", Int64.Type}, {"Q173I", Int64.Type}, {"Q173E", Int64.Type}, {"Q174A", Int64.Type}, {"Q174I", Int64.Type}, {"Q174E", Int64.Type}, {"Q175A", Int64.Type}, {"Q175I", Int64.Type}, {"Q175E", Int64.Type}, {"Q176A", Int64.Type}, {"Q176I", Int64.Type}, {"Q176E", Int64.Type}, {"Q177A", Int64.Type}, {"Q177I", Int64.Type}, {"Q177E", Int64.Type}, {"Q178A", Int64.Type}, {"Q178I", Int64.Type}, {"Q178E", Int64.Type}, {"Q179A", Int64.Type}, {"Q179I", Int64.Type}, {"Q179E", Int64.Type}, {"Q180A", Int64.Type}, {"Q180I", Int64.Type}, {"Q180E", Int64.Type}, {"Q181A", Int64.Type}, {"Q181I", Int64.Type}, {"Q181E", Int64.Type}}),
    
    #"Added Column ""User""" = Table.AddIndexColumn(#"Changed Type", "User", 1, 1, Int64.Type),
    
    #"Reordered Columns" = Table.ReorderColumns(#"Added Column ""User""",{"User", "Q1A", "Q1I", "Q1E", "Q2A", "Q2I", "Q2E", "Q3A", "Q3I", "Q3E", "Q4A", "Q4I", "Q4E", "Q5A", "Q5I", "Q5E", "Q6A", "Q6I", "Q6E", "Q7A", "Q7I", "Q7E", "Q8A", "Q8I", "Q8E", "Q9A", "Q9I", "Q9E", "Q10A", "Q10I", "Q10E", "Q11A", "Q11I", "Q11E", "Q12A", "Q12I", "Q12E", "Q13A", "Q13I", "Q13E", "Q14A", "Q14I", "Q14E", "Q15A", "Q15I", "Q15E", "Q16A", "Q16I", "Q16E", "Q17A", "Q17I", "Q17E", "Q18A", "Q18I", "Q18E", "Q19A", "Q19I", "Q19E", "Q20A", "Q20I", "Q20E", "Q21A", "Q21I", "Q21E", "Q22A", "Q22I", "Q22E", "Q23A", "Q23I", "Q23E", "Q24A", "Q24I", "Q24E", "Q25A", "Q25I", "Q25E", "Q26A", "Q26I", "Q26E", "Q27A", "Q27I", "Q27E", "Q28A", "Q28I", "Q28E", "Q29A", "Q29I", "Q29E", "Q30A", "Q30I", "Q30E", "Q31A", "Q31I", "Q31E", "Q32A", "Q32I", "Q32E", "Q33A", "Q33I", "Q33E", "Q34A", "Q34I", "Q34E", "Q35A", "Q35I", "Q35E", "Q36A", "Q36I", "Q36E", "Q37A", "Q37I", "Q37E", "Q38A", "Q38I", "Q38E", "Q39A", "Q39I", "Q39E", "Q40A", "Q40I", "Q40E", "Q41A", "Q41I", "Q41E", "Q42A", "Q42I", "Q42E", "Q43A", "Q43I", "Q43E", "Q44A", "Q44I", "Q44E", "Q45A", "Q45I", "Q45E", "Q46A", "Q46I", "Q46E", "Q47A", "Q47I", "Q47E", "Q48A", "Q48I", "Q48E", "Q49A", "Q49I", "Q49E", "Q50A", "Q50I", "Q50E", "Q51A", "Q51I", "Q51E", "Q52A", "Q52I", "Q52E", "Q53A", "Q53I", "Q53E", "Q54A", "Q54I", "Q54E", "Q55A", "Q55I", "Q55E", "Q56A", "Q56I", "Q56E", "Q57A", "Q57I", "Q57E", "Q58A", "Q58I", "Q58E", "Q59A", "Q59I", "Q59E", "Q60A", "Q60I", "Q60E", "Q61A", "Q61I", "Q61E", "Q62A", "Q62I", "Q62E", "Q63A", "Q63I", "Q63E", "Q64A", "Q64I", "Q64E", "Q65A", "Q65I", "Q65E", "Q66A", "Q66I", "Q66E", "Q67A", "Q67I", "Q67E", "Q68A", "Q68I", "Q68E", "Q69A", "Q69I", "Q69E", "Q70A", "Q70I", "Q70E", "Q71A", "Q71I", "Q71E", "Q72A", "Q72I", "Q72E", "Q73A", "Q73I", "Q73E", "Q74A", "Q74I", "Q74E", "Q75A", "Q75I", "Q75E", "Q76A", "Q76I", "Q76E", "Q77A", "Q77I", "Q77E", "Q78A", "Q78I", "Q78E", "Q79A", "Q79I", "Q79E", "Q80A", "Q80I", "Q80E", "Q81A", "Q81I", "Q81E", "Q82A", "Q82I", "Q82E", "Q83A", "Q83I", "Q83E", "Q84A", "Q84I", "Q84E", "Q85A", "Q85I", "Q85E", "Q86A", "Q86I", "Q86E", "Q87A", "Q87I", "Q87E", "Q88A", "Q88I", "Q88E", "Q89A", "Q89I", "Q89E", "Q90A", "Q90I", "Q90E", "Q91A", "Q91I", "Q91E", "Q92A", "Q92I", "Q92E", "Q93A", "Q93I", "Q93E", "Q94A", "Q94I", "Q94E", "Q95A", "Q95I", "Q95E", "Q96A", "Q96I", "Q96E", "Q97A", "Q97I", "Q97E", "Q98A", "Q98I", "Q98E", "Q99A", "Q99I", "Q99E", "Q100A", "Q100I", "Q100E", "Q101A", "Q101I", "Q101E", "Q102A", "Q102I", "Q102E", "Q103A", "Q103I", "Q103E", "Q104A", "Q104I", "Q104E", "Q105A", "Q105I", "Q105E", "Q106A", "Q106I", "Q106E", "Q107A", "Q107I", "Q107E", "Q108A", "Q108I", "Q108E", "Q109A", "Q109I", "Q109E", "Q110A", "Q110I", "Q110E", "Q111A", "Q111I", "Q111E", "Q112A", "Q112I", "Q112E", "Q113A", "Q113I", "Q113E", "Q114A", "Q114I", "Q114E", "Q115A", "Q115I", "Q115E", "Q116A", "Q116I", "Q116E", "Q117A", "Q117I", "Q117E", "Q118A", "Q118I", "Q118E", "Q119A", "Q119I", "Q119E", "Q120A", "Q120I", "Q120E", "Q121A", "Q121I", "Q121E", "Q122A", "Q122I", "Q122E", "Q123A", "Q123I", "Q123E", "Q124A", "Q124I", "Q124E", "Q125A", "Q125I", "Q125E", "Q126A", "Q126I", "Q126E", "Q127A", "Q127I", "Q127E", "Q128A", "Q128I", "Q128E", "Q129A", "Q129I", "Q129E", "Q130A", "Q130I", "Q130E", "Q131A", "Q131I", "Q131E", "Q132A", "Q132I", "Q132E", "Q133A", "Q133I", "Q133E", "Q134A", "Q134I", "Q134E", "Q135A", "Q135I", "Q135E", "Q136A", "Q136I", "Q136E", "Q137A", "Q137I", "Q137E", "Q138A", "Q138I", "Q138E", "Q139A", "Q139I", "Q139E", "Q140A", "Q140I", "Q140E", "Q141A", "Q141I", "Q141E", "Q142A", "Q142I", "Q142E", "Q143A", "Q143I", "Q143E", "Q144A", "Q144I", "Q144E", "Q145A", "Q145I", "Q145E", "Q146A", "Q146I", "Q146E", "Q147A", "Q147I", "Q147E", "Q148A", "Q148I", "Q148E", "Q149A", "Q149I", "Q149E", "Q150A", "Q150I", "Q150E", "Q151A", "Q151I", "Q151E", "Q152A", "Q152I", "Q152E", "Q153A", "Q153I", "Q153E", "Q154A", "Q154I", "Q154E", "Q155A", "Q155I", "Q155E", "Q156A", "Q156I", "Q156E", "Q157A", "Q157I", "Q157E", "Q158A", "Q158I", "Q158E", "Q159A", "Q159I", "Q159E", "Q160A", "Q160I", "Q160E", "Q161A", "Q161I", "Q161E", "Q162A", "Q162I", "Q162E", "Q163A", "Q163I", "Q163E", "Q164A", "Q164I", "Q164E", "Q165A", "Q165I", "Q165E", "Q166A", "Q166I", "Q166E", "Q167A", "Q167I", "Q167E", "Q168A", "Q168I", "Q168E", "Q169A", "Q169I", "Q169E", "Q170A", "Q170I", "Q170E", "Q171A", "Q171I", "Q171E", "Q172A", "Q172I", "Q172E", "Q173A", "Q173I", "Q173E", "Q174A", "Q174I", "Q174E", "Q175A", "Q175I", "Q175E", "Q176A", "Q176I", "Q176E", "Q177A", "Q177I", "Q177E", "Q178A", "Q178I", "Q178E", "Q179A", "Q179I", "Q179E", "Q180A", "Q180I", "Q180E", "Q181A", "Q181I", "Q181E"})
in
    #"Reordered Columns"
```

## Limitations

- No demographic data (age, gender, etc.)

- Self-reported data, subject to response biases

- No external validity checks

## License

The dataset is available under the terms described by OpenPsychometrics. This dashboard is for educational and research purposes only.

