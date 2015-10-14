<!--
item|setting
---|---
UUID|Use save UUID for your site
Major|Major should be identical for all beacons on one floor (at least nearby beacons for each edge in a route).
Minor|Use unique id in Major group
Power|-8db or -12db is recommended


![Good/NG beacon location example](images/beacon_location.png)

Another data set collected in the same way (“test data”) is needed for [Accuracy Evaluation](#acc_eval) i.e. evaluating the accuracy of localization based on the model. This subsection describes the procedures for fingerprinting, accuracy evaluation and how to make an input file importable to your navigation application.

1. open "Settings" app

    ![Settings app icon](images/settings_icon.png)

2. You can find "NavCog"
3. Set "Developer Mode" on

    ![NavCog preference view](images/pref_view.png)

Now you can see "**Get Data**" button in the bottom of the NavCog main view.


###[Fingerprinting] For each points to be fingerprinted,
![Sampling tips in fingerprinting](images/sampling_method.png)


## Preprocess Data

### Extract data from iPhone
1. connect iPhone to your Mac (USB)
2. open iTunes and select your device listed in iTunes
3. select apps view

    ![Apps menu image](./images/apps_menu.png)
4. Find "NavCog" app in the Apps list of File Shareing section
5. Create a folder (i.e. "**train**") at your Desktop
5. 	Select all files with name "**edge-xxx.json**" and click "**Save**" button to save files in to the folder

### Concatinate individual files

1. Open **Terminal** app
2. Type the following command

   ```
   cd Desktop
   curl -o concat.sh https://navcog.mybluemix.net/tools/data-merge.sh
   sh data-merge.sh train
   ```


1. Prepare **train-data** and **test-data** on your desktop (both folder has concatinated data files)

  You can take test data by the same procedure for training data. (**Note**: App always save fingerprinting data in edge-<edge id>-<x>-<y>.txt. You should extract all training data before start taking test data. It is easy if you have two iPhones for development)
3. Type the following command

   ```
   cd Desktop
   curl -o TestAccuracy https://navcog.mybluemix.net/tools/TestAccuracy
   TestAccuracy training-data test-data > result.json
   ```

  **aveError** shows average error of localization. Usually you can get less than about 7 feet.

  ```
{
  "unit": 0.9144,
  "results": [
    {
      "edgeID": 1,
      "testSmpNum": 1260,
      "aveDist": 80.1254,
      "aveDistNorm": 0.0958437,
      "maxDist": 836,
      "minDist": 0,
      "worstPos": {
        "x": 0,
        "y": 10
      },
      "aveError": 4.26773,
      "maxError": 22.7845,
      "minError": 0,
      "prob1U": 0.456349,
      "prob2U": 0.746825,
      "prob3U": 0.898413
    }
  ],
  "description": {
    "aveDist": "Average Distance",
    "aveDistNorm": "Normalized Average Distance",
    "maxDist": "Maximum Distance",
    "minDist": "Minimum Distance",
    "worstPos": "Worst Position",
    "aveError": "Average Error (feet)",
    "maxError": "Maximum Error (feet)",
    "minError": "Minimum Error (feet)",
    "prob1U": "Probability of less than 3 feet (0.9144 meter)",
    "prob2U": "Probability of less than 6 feet (1.8288 meter)",
    "prob3U": "Probability of less than 9 feet (2.7432 meter)"
  }
}
  ```



```
```