---
sort: 4
title: Operator Documentation
---

## <a name="dashboard"></a>Dashboard

* The Dashboard displays the current state of the system by showing a number of status parameters related to wagons, trains and grid jobs. Additionally, the user can see the summary of the last week: average completion time, number of finished trains and wagon tests. 
* Below, a display of the grid jobs state during the previous week is displayed, for every site.

<div align="center">
<img src="../images/dashboard.png" width="90%">
</div>

* By clicking the number of wagons waiting to be included in a train, the user can directly open the [_Train Submission_](#trainsubmission). Similarly, a link to the [_Train Runs_](#trainruns) is available by clicking the number of trains to be submitted to the grid, the number of running tests, or the number of finished trains.

## <a name="trainsubmission"></a>Train Submission
* For a user, the _Train Submission_ page displays a read view only of datasets which have enabled wagons.
* For a train operator, the  _Train Submission_ page displays only datasets which have enabled wagons, and allows train composition, as well as submitting, modifying and killing a train.
### <a name="trainsubmission"></a>Train Composition
* Trains are composed per dataset. Only wagons which have a test status of success `🌟` or warning `❗️` can be composed in a train.
* By default, wagons that were enabled at most one week ago are shown. In order to display all enabled wagons, click on `off` in the _Enabled_ column.
* In order to compose a train, click `☑️` in the _Compose_ column. The package tag will be automatically chosen, and other wagons that can be included in the train run are signalized with 🟢, and the ones which are not compatible with 🔴. All wagons that are compatible can be automatically chosen by clicking on `✅ Select all compatible wagons`, or by selecting them one by one.
* `☑️ automatic composition`: Train composition schedule is defined in the dataset settings. If the dataset has a defined schedule, the trains will be automatically composed at the specified time if the tests have finished without a warning.

<div align="center">
<img src="../images/automaticComposition.png" width="90%">
</div>

* If a wagon has _Derived data_ tables activated, it will be signalized with the icon 🗂️ in the _Test status_ column. 
* `☑️ slow train`: If enabled, the express train features are disabled. This means that you may have up to 2% more jobs which finish but the train run may take several days more.
* `☑️ derived data`: If enabled, this train produces derived data to be used for further analysis. The results will not be merged and can be used as input for future train runs.
* `☑️ automatic submission`: If enabled will submit the train automatically after the test is done and succeds `🌟`.
* `Target`: Sets the facility/cores where the train will be run.
* Finally, after defining the configuration, click on `Compose 🚂` to compose a train. After composing a train run, the wagons selected cannot be selected for a different train run unless the current train run is [decomposed](#decompose). After the train run is [submitted](#submit), the wagons will be disabled. 
* If a user changes a configuration between train composition and submission, the new configuration is not taken into account. The train runs with the wagons and dataset configuration corresponding to the time at which it was _created_.
* The train will be automatically tested, and its progress can be followed in the _Train Runs_ table, or in the [**Train Runs**](#trainruns) page by clicking on the TRAIN_ID link.

### <a name="stagedsubmission"></a>Staged Submission

* Linked datasets are subsets of a big dataset
* First, a train run needs to be _Done_ on a smaller linked dataset before being run on a bigger dataset
* Any user who is part of the analysis can **request a long train**
* Approval from the participating analyses PWGs conveners is required in order to submit a long train 

<div align="center">
<img src="../images/requestLongTrain.png" width="90%">
</div>

## <a name="trainruns"></a>Train Runs
* For a user, the _Train Runs_ page displays a read view only of all train runs available in the system.
* For a train operator, the _Train Runs_ page displays all trains available in the system, and allows submitting, modifying and killing a train.
* The train run detail can be accessed by clicking on the TRAIN_ID, or with the url <a href="https://alimonitor.cern.ch/hyperloop/train-run/TRAIN_ID" target="_blank">https://alimonitor.cern.ch/hyperloop/train-run/TRAIN_ID</a>.
* The actions allowed in a train run:
  * Adding a _comment_.
  * <a name="decompose"></a>Decomposing a train run by clicking on `Decompose ☠️`. When decomposing a train run, the wagons in it will be allowed to be composed in a new train run.
  * <a name="submit"></a>Submitting a train run. Only train runs which test status is success `🌟` or warning `❗️` can be submitted. By default, the train run is submitted to the _Grid - single core_, but a different target can be chosen. To submit a train run, click on `Submit 🚂`.
  * <a name="submitfinalmerge"></a>When a dataset has activate final merge, a button `Submit final merge` appears after all the masterjobs are finished successfully.
  * Killing a train by clicking on `Kill ☠️`.
  * Cloning a train in the tab `Clone`. When cloning a train, the wagons and dataset configuration used will be the same as of the original train. Other settings can be changed: package tag, target facility, slow train, derived data, automatic submission.
* In the _Test Graphs_ tab, the user can plot the available metrics for the specific _Train run_. By hovering over the graph, the corresponding values are displayed in a dynamic window, stating the value for each participating wagon.

  <div align="center">
    <img src="../images/testGraphs.png" width="70%">
  </div>

  * The metric can be selected from the upper-left dropdown, and the graph will change accordingly.
  * The user can choose to plot the metric data per device, by checking the _Per Device_ near the dropdown.


  <div align="center">
    <img src="../images/testGraphsPerDevice.png" width="70%">
  </div>


  * In order to plot the highest ten graphs, that means the graphs with the highest average, the user can click the **Show top 10 largest** checkbox.
  
  
  <div align="center">
    <img src="../images/graphLargest.png" width="70%">
  </div>
  
  * The user can zoom into the graph by clicking and dragging the mouse along the area of interest. For zooming out, the user must double-click on the graph.


  <div align="center">
    <img src="../images/graphZoom.png" width="70%">
  </div>
  
   <div align="center">
    <img src="../images/graphZoom2.png" width="70%">
   </div>
  
  
## <a name="trainswithissues"></a>Trains with issues

* This page displays the list of train runs that are in state _submitted_ but could not be merged due to specific problems:
  * More than 30% of the jobs have errors 
  * The Analysis job has all jobs in a final state, but there is no merging job
  * There is a merging job which is in a final state, but there is no final merge job
  * There is a final merge job in final state, but the merging is not declared as _done_ in the database


  <div align="center">
    <img src="../images/trainsWithIssues.png" width="70%">
  </div>
  
  
* The operator must analyse this cases and decide upon resubmitting some of the jobs, launching the final merging submission where the errors are not significant, or killing the train when there are too many errors.
  
## <a name="datasets"></a>Datasets

* Displays all the datasets available. The operator can add, remove, activate or deactivate a dataset. Likewise, by clicking the `📝` button, the operator is able to modify the dataset in the Edit Dataset page.

  <div align="center">
    <img src="../images/datasetsPage.png" width="100%">
  </div>

* The runlists will be received programtically from the DPG.

## <a name="dpgrunlist"></a>DPG Runlists

* This page is dedicated to the DPG experts and displayes all the DPG runlists created for the datasets. The DPG expert can add, edit or remove a runlist.

 <div align="center">
    <img src="../images/DPGrunlists.png" width="100%">
  </div>
  
* Clicking on the the `📝` button will lead to the edit view, where the DPG expert can change the list of runs.

 <div align="center">
    <img src="../images/editRunlist.png" width="70%">
  </div>
  
* DPG experts can create a new runlist by clikcing the **+Add runlist** button. In order to create the list of runs, the correct data type, anchor, tag and production must be selected.

 <div align="center">
    <img src="../images/addRunlist.png" width="70%">
  </div>

