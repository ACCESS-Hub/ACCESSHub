{% set model = "ACCESS-OM2" %}
{% set github_configs = "https://github.com/ACCESS-NRI/access-om2-configs" %}
[cosima]: https://cosima.org.au
[PBS job]: https://opus.nci.org.au/display/Help/4.+PBS+Jobs
[payu]: https://github.com/payu-org/payu
[model components]: /models/configurations/access-om/#model-components
[model configurations]: /models/configurations/access-om/#access-om2
[gadi]: https://opus.nci.org.au/display/Help/0.+Welcome+to+Gadi#id-0.WelcometoGadi-Overview

[:fontawesome-brands-github:{: class="twemoji icon-before-text"} {{ model }} configurations]({{github_configs}}){: class="text-card"}

# Run {{ model }}

## About

{{ model }} is an Ocean Sea-Ice model. More information is available in the [{{ model }} overview][model configurations].

The instructions below outline how to run {{ model }} using ACCESS-NRI's software deployment pipeline, specifically designed to run on the [National Computating Infrastructure (NCI)](https://nci.org.au/about-us/who-we-are) supercomputer [_Gadi_][gadi].

If you are unsure whether {{ model }} is the right choice for your experiment, take a look at the overview of [ACCESS Models](/models).

All {{model}} configurations are open source, licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/?ref=chooser-v1")![CC icon](https://mirrors.creativecommons.org/presskit/icons/cc.svg?ref=chooser-v1){: style="height:1em;margin-left:0.2em;vertical-align:text-top;"}![BY icon](https://mirrors.creativecommons.org/presskit/icons/by.svg?ref=chooser-v1){: style="height:1em;margin-left:0.2em;vertical-align:text-top;"} and available on [ACCESS-NRI GitHub]({{github_configs}}).

{{ model }} release notes are [available on the ACCESS-Hive Forum](https://forum.access-hive.org.au/t/access-om2-release-information/1602/) and are updated when new releases are made available.

## Prerequisites

### General prerequisites

Before running {{ model }}, you need to [Set Up your NCI Account](/getting_started/set_up_nci_account).

### Model-specific prerequisites

- **Join the _vk83_ and _qv56_ projects at NCI**<br>
    To join these projects, request membership on the respective [vk83](https://my.nci.org.au/mancini/project/vk83/join) and [qv56](https://my.nci.org.au/mancini/project/qv56/join) NCI project pages.<br>
    For more information on joining specific NCI projects, refer to [How to connect to a project](https://opus.nci.org.au/display/Help/How+to+connect+to+a+project).

- **Payu**<br>
    [Payu][payu] is a workflow management tool for running numerical models in supercomputing environments, for which there is extensive [documentation](https://payu.readthedocs.io/en/latest/).<br>
    _Payu_ on _Gadi_ is available through a dedicated `conda` environment in the _vk83_ project.<br>
    After joining the _vk83_ project, load the `payu` module:
    
        module use /g/data/vk83/modules
        module load payu

    To check that _payu_ is available, run:

        payu --version
    
    <terminal-window>
        <terminal-line data="input">payu --version</terminal-line>
        <terminal-line lineDelay="1000">1.1.3</terminal-line>
    </terminal-window>
    !!! warning
        _payu_ version >=1.1.3 is required

----------------------------------------------------------------------------------------

## Get {{ model }} configuration

All released {{ model }} configurations are available from the [{{ model }} configs]({{ github_configs }}) GitHub repository.<br>
Released configurations are tested and supported by ACCESS-NRI, as an adaptation of those originally developed by [COSIMA][cosima].

For more information on {{ model }} configurations, check [{{model}}][model configurations] page.

More information about the available experiments and the naming scheme of the branches can also be found in the [{{ model }} configs]({{ github_configs }}) GitHub repository.

The first step is to choose a configuration from those available.<br>
For example, if the required configuration is the 1° horizontal resolution with repeat-year _JRA55_ forcing (without BGC), then the branch to select is [`release-1deg_jra55_ryf`](https://github.com/ACCESS-NRI/access-om2-configs/tree/release-1deg_jra55_ryf).

To clone this branch to a location on _Gadi_ and navigate to that directory, run:
    
    mkdir -p ~/access-om2
    cd ~/access-om2
    payu clone -b expt -B release-1deg_jra55_ryf {{ github_configs }} 1deg_jra55_ryf
    cd 1deg_jra55_ryf

In the example above the `payu clone` command clones the 1° repeat-year JRA55 configuration (` -B release-1deg_jra55_ryf`) 
as a new experiment branch (`-b expt`) to a directory named `1deg_jra55_ryf`.
!!! admonition tip
    Anyone using a configuration is advised to clone only a single branch (as shown in the example above) and not the entire repository.

<terminal-window>
    <terminal-line data="input">mkdir -p ~/access-om2</terminal-line>
    <terminal-line data="input">cd ~/access-om2</terminal-line>
    <terminal-line data="input" directory="~/access-om2">payu clone -b expt -B release-1deg_jra55_ryf https://github.com/ACCESS-NRI/access-om2-configs.git 1deg_jra55_ryf</terminal-line>
    <terminal-line lineDelay=1000>Cloned repository from https://github.com/ACCESS-NRI/access-om2-configs.git to directory: .../access-om/1deg_jra55_ryf</terminal-line>
    <terminal-line>Created and checked out new branch: expt</terminal-line>
    <terminal-line>laboratory path:  /scratch/.../access-om2</terminal-line>
    <terminal-line>binary path:  /scratch/.../access-om2/bin</terminal-line>
    <terminal-line>input path:  /scratch/.../access-om2/input</terminal-line>
    <terminal-line>work path:  /scratch/.../access-om2/work</terminal-line>
    <terminal-line>archive path:  /scratch/.../access-om2/archive</terminal-line>
    <terminal-line>Updated metadata. Experiment UUID: daeee7ff-07e4-4f93-823b-cb7c6e4bdb6e</terminal-line>
    <terminal-line>Added archive symlink to /scratch/.../access-om2/archive/1deg_jra55_ryf-expt-daeee7ff</terminal-line>
    <terminal-line data="input">cd 1deg_jra55_ryf</terminal-line>
</terminal-window>

!!! tip
    _payu_ uses branches to differentiate between different experiments in the same local git repository.<br>
    For this reason, it is recommended to always set the cloned branch name (`expt` in the example above) to something meaningful for the planned experiment.<br>
    For more information refer to this [payu tutorial](https://forum.access-hive.org.au/t/access-om2-payu-tutorial/1750#select-experiment-12).

----------------------------------------------------------------------------------------

## Run {{ model }} configuration

If you want to modify your configuration, refer to [Edit {{ model }} configuration](#edit-{{ model.lower() }}-configuration).

{{ model }} configurations run on [_Gadi_][gadi] through a [PBS job][PBS job] submission managed by [_payu_][payu].

The general layout of a _payu_-supported model run consists of two main directories:

- The _control_ directory contains the model configuration and serves as the execution directory for running the model (in this example, the cloned directory `~/access-om2/1deg_jra55_ryf`).
- The _laboratory_ directory, where all the model components reside. For {{ model }}, it is typically `/scratch/$PROJECT/$USER/access-om2`.

This separates the small text configuration files from the larger binary outputs and inputs. In this way, the _control_ directory can be in the `$HOME` directory (as it is the only filesystem actively backed-up on _Gadi_). The quotas for `$HOME` are low and strict, which limits what can be stored there, so it is not suitable for larger files.

The _laboratory_ directory is a shared space for all _payu_ experiments using the same model.<br>
Inside the _laboratory_ directory there are two subdirectories:

- `work` &rarr; a directory where _payu_ automatically creates a temporary subdirectory while the model is run. The temporary subdirectory gets created as part of a run and then removed after the run succeeds.
- `archive` &rarr; the directory where the output is stored following each successful run.

Within each of the above directories _payu_ automatically creates subdirectories uniquely named according to the experiment being run.<br>
_Payu_ also creates symbolic links in the _control_ directory pointing to the `archive` and `work` directories.

This design allows multiple self-resubmitting experiments that share common executables and input data to be run simultaneously.

!!! admonition warning
    Files on the `/scratch` drive, such as the _laboratory_ directory, might get deleted if not accessed for several days and the `/scratch` drive is limited in space. For these reasons, all model runs which are to be kept should be moved to `/g/data/` by enabling the _sync_ step in _payu_. To know more refer to [Syncing output data](#syncing-output-data).

### Run configuration

To run {{ model }} configuration execute the following command from within the *control* directory:

    payu run

This will submit a single job to the queue with a run length of `restart_period`.<br>
For information about `restart_period`, refer to [Change run length](#change-run-length).

<terminal-window>
    <terminal-line data="input">cd ~/access-om2/1deg_jra55_ryf</terminal-line>
    <terminal-line directory="~/access-om2/1deg_jra55_ryf" data="input">payu run</terminal-line>
    <terminal-line>payu: warning: Job request includes 47 unused CPUs.</terminal-line>
    <terminal-line lineDelay=50>payu: warning: CPU request increased from 241 to 288</terminal-line>
    <terminal-line lineDelay=50>Loading input manifest: manifests/input.yaml</terminal-line>
    <terminal-line lineDelay=50>Loading restart manifest: manifests/restart.yaml</terminal-line>
    <terminal-line lineDelay=50>Loading exe manifest: manifests/exe.yaml</terminal-line>
    <terminal-line lineDelay=50>payu: Found modules in /opt/Modules/v4.3.0</terminal-line>
    <terminal-line lineDelay=50>
        qsub -q normal -P tm70 -l walltime=10800 -l ncpus=288 -l mem=1000GB -N 1deg_jra55_ryf -l wd -j n -v PAYU_PATH=/g/data/hh5/public/apps/miniconda3/envs/analysis3-23.10/bin,MODULESHOME=/opt/Modules/v4.3.0,MODULES_CMD=/opt/Modules/v4.3.0/libexec/modulecmd.tcl,MODULEPATH=/g/data/hr22/modulefiles:/g/data/hh5/public/modules:/etc/scl/modulefiles:/opt/Modules/modulefiles:/opt/Modules/v4.3.0/modulefiles:/apps/Modules/modulefiles -W umask=027 -l storage=gdata/hh5+gdata/vk83 -- /g/data/hh5/public/apps/miniconda3/envs/analysis3-23.10/bin/python3.10 /g/data/hh5/public/apps/miniconda3/envs/analysis3-23.10/bin/payu-run
    </terminal-line>
    <terminal-line lineDelay=50>&lt;job-ID&gt;.gadi-pbs</job-ID>></terminal-line>
</terminal-window>
!!! tip
    You can add the `-f` option to `payu run` to let the model run even if there is an existing non-empty `work` directory, created from a previous failed run or from running `payu setup`.

----------------------------------------------------------------------------------------

## Monitor {{ model }} runs

The `payu run` command prints out the PBS `job-ID` (formatted as `<9-digit-number>.gadi-pbs`), as the last line to the terminal.<br>
To print out information on the status of a specific job, you can execute the following command:
```
qstat <job-ID>
```
<terminal-window>
    <terminal-line data="input">qstat &lt;job-ID&gt;</terminal-line>
    <terminal-line linedelay=500>Job id&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Name&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;User&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Time Use&nbsp;S Queue</terminal-line>
    <terminal-line linedelay=0>---------------------  ---------------- ----------------  -------- - -----</terminal-line>
    <terminal-line linedelay=0>&lt;job-ID&gt;.gadi-pbs&nbsp;&nbsp;1deg_jra55_ryf&nbsp;&nbsp;&nbsp;&lt;$USER&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;time&gt;&nbsp;R&nbsp;normal-exec</terminal-line>
</terminal-window>

To show the status of all your submitted [PBS jobs][PBS job], you can execute the following command:
```
qstat -u $USER
```

<terminal-window>
    <terminal-line data="input">qstat -u $USER</terminal-line>
    <terminal-line linedelay=500>Job id&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Name&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;User&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Time Use&nbsp;S Queue</terminal-line>
    <terminal-line linedelay=0>---------------------  ---------------- ----------------  -------- - -----</terminal-line>
    <terminal-line linedelay=0>&lt;job-ID&gt;.gadi-pbs&nbsp;&nbsp;&nbsp;1deg_jra55_ryf&nbsp;&nbsp;&nbsp;&lt;$USER&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;time&gt;&nbsp;R&nbsp;normal-exec</terminal-line>
    <terminal-line linedelay=0>&lt;job-ID&gt;.gadi-pbs&nbsp;&nbsp;&nbsp;&lt;other-job-name&gt;&nbsp;&lt;$USER&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;time&gt;&nbsp;R&nbsp;normal-exec</terminal-line>
    <terminal-line linedelay=0>&lt;job-ID&gt;.gadi-pbs&nbsp;&nbsp;&nbsp;&lt;other-job-name&gt;&nbsp;&lt;$USER&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;time&gt;&nbsp;R&nbsp;normal-exec</terminal-line>
</terminal-window>

The default name of your job is the name of the _payu_ _control_ directory (`1deg_jra55_ryf` in the example above).<br>
This can be changed by altering the `jobname` in the [PBS resources section](#modify-pbs-resources) of the `config.yaml` file.

_S_ indicates the status of your run, where:

- _Q_ &rarr; Job waiting in the queue to start
- _R_ &rarr; Job running
- _E_ &rarr; Job ending
- _H_ &rarr; Job on hold

If there are no jobs listed with your `jobname` (or if no job is listed), your run either successfully completed or was terminated due to an error.<br>
For more information, check [NCI documentation](https://opus.nci.org.au/display/Help/FAQ+1%3A+Why+My+Jobs+are+NOT+Running).

### Stop a run

If you want to manually terminate a run, you can do so by executing:
```
qdel <job-ID>
```
which kills the specified job without waiting for it to complete.

!!! tip
    If you started an {{ model }} run using the `-n` option (e.g., to [run the model for more than 5 years](#run-configuration-for-more-than-5-years)), but subsequently decide not to keep running after the current process completes, you can create a file called `stop_run` in the _control_ directory.<br>
    This will prevent _payu_ from submitting another job.

### Error and output log files

#### PBS output files
When the model completes a run, PBS writes the standard output and error streams to two files inside the _control_ directory: `<jobname>.o<job-ID>` and `<jobname>.e<job-ID>`, respectively.

These files usually contain logs about _payu_ tasks, and give an overview of the resources used by the job.<br>
To move these files to the `archive` directory, use the following commmand:
```
payu sweep
```

#### Model log files

While the model is running, _payu_ saves the model standard output and error streams in the `access-om2.out` and `access-om2.err` files inside the _control_ directory, respectively.<br>
You can examine the contents of these files to check on the status of a run as it progresses (or after a failed run has completed).

!!! warning
    At the end of a successful run these log files are archived to the `archive` directory and will no longer be found in the _control_ directory. If they remain in the _control_ directory after the PBS job for a run has completed it means the run has failed.

### Model Live Diagnostics

ACCESS-NRI developed the [Model Live Diagnostics](/model_evaluation/evaluation_on_gadi/model_live_diagnostics) framework to check, monitor, visualise, and evaluate model behaviour and progress of ACCESS models currently running on _Gadi_.<br>
For a complete documentation on how to use this framework, check the [Model Diagnostics documentation](https://med-live-diagnostics.readthedocs.io/en/latest/index.html).

### Trouble-shooting

If _payu_ doesn't run correctly for some reason, a good first step is to run the following command from within the _control_ directory:

    payu setup

This command will: 
  
  - create the _laboratory_ and `work` directories based on the experiment configuration
  - generate manifests
  - report useful information to the user, such as the location of the _laboratory_ where the `work` and `archive` directories are located

<terminal-window>
    <terminal-line data="input">payu setup</terminal-line>
    <terminal-line>laboratory path: /scratch/$PROJECT/$USER/access-om2</terminal-line>
    <terminal-line>binary path: /scratch/$PROJECT/$USER/access-om2/bin</terminal-line>
    <terminal-line>input path: /scratch/$PROJECT/$USER/access-om2/input</terminal-line>
    <terminal-line>work path: /scratch/$PROJECT/$USER/access-om2/work</terminal-line>
    <terminal-line>archive path: /scratch/$PROJECT/$USER/access-om2/archive</terminal-line>
    <terminal-line>Loading input manifest: manifests/input.yaml</terminal-line>
    <terminal-line>Loading restart manifest: manifests/restart.yaml</terminal-line>
    <terminal-line>Loading exe manifest: manifests/exe.yaml</terminal-line>
    <terminal-line>Setting up atmosphere</terminal-line>
    <terminal-line>Setting up ocean</terminal-line>
    <terminal-line>Setting up ice</terminal-line>
    <terminal-line>Setting up access-om2</terminal-line>
    <terminal-line>Checking exe and input manifests</terminal-line>
    <terminal-line>Updating full hashes for 3 files in manifests/exe.yaml</terminal-line>
    <terminal-line>Creating restart manifest</terminal-line>
    <terminal-line>Writing manifests/restart.yaml</terminal-line>
    <terminal-line>Writing manifests/exe.yaml</terminal-line>
</terminal-window>

This can help to isolate issues such as permissions problems accessing files and directories, missing files or malformed/incorrect paths.

----------------------------------------------------------------------------------------

## {{ model }} outputs

At the end of a successful model run, output files, restart files and log files are moved from the `work` directory to the `archive` directory.<br>
Symbolic links to these directories are also provided in the _control_ directory for convenience.

If a model run is unsuccessful, the `work` directory is left untouched to facilitate the identification of the cause of the model failure.

Outputs and restarts are stored in subfolders within the `archive` directory, subdivided for each run of the model.<br>
Output and restart folders are called `outputXXX` and `restartXXX`, respectively, where _XXX_ is the run number starting from `000`.

Model components are separated into subdirectories within the output and restart directories.

<terminal-window>
    <terminal-line data="input">cd ~/access-om2/1deg_jra55_ryf</terminal-line>
    <terminal-line data="input" directory="~/access-om2/1deg_jra55_yaf<">ls</terminal-line>
    <terminal-line class="ls-output-format">output000 pbs_logs restart000</terminal-line>
</terminal-window>

----------------------------------------------------------------------------------------

## Edit {{ model }} configuration

This section describes how to modify {{ model }} configuration.<br>
The modifications discussed in this section can change the way {{ model }} is run by _payu_, or how its specific [model components] are configured and coupled together.

The `config.yaml` file located in the _control_ directory is the _Master Configuration_ file, which controls the general model configuration. It contains several parts, some of which it is more likely will need modification, and others which are rarely changed without having a deep understanding of how the model is configured.

To find out more about configuration settings for the `config.yaml` file, refer to [how to configure your experiment with payu](https://payu.readthedocs.io/en/latest/config.html).

### Change run length

One of the most common changes is to adjust the duration of the model run.<br>
For example, when debugging changes to a model, it is common to reduce the run length to minimise resource consumption and return faster feedback on changes.

The run length is controlled by the `restart_period` field in the `&date_manager_nml` section of the `~/access-om2/1deg_jra55_ryf/accessom2.nml` file:

    &date_manager_nml
        forcing_start_date = '1958-01-01T00:00:00'
        forcing_end_date = '2019-01-01T00:00:00'<br>
        ! Runtime for a single segment/job/submit, format is years, months, seconds,
        ! two of which must be zero.
        restart_period = 5, 0, 0
    &end

The format is `restart_period = <number_of_years>, <number_of_months>, <number_of_days>`.

For example, to make the model run for 1 year, 4 months and 10 days, change `restart_period` to:

    restart_period = 1, 4, 10

!!! warning
    While `restart_period` can be reduced, it should not be increased to more than 5 years to avoid errors.
    <br><br>
    It is also important to differentiate between _run length_ and _total experiment length_.<br>
    For more information about their difference, or how to run the model for more than 5 years, refer to [Run configuration for more than 5 years](#run-configuration-for-more-than-5-years).

#### Run configuration for more than 5 years

As mentioned in the [Change run length](#change-run-length) section, you cannot specify more than 5 years as `restart_period`.<br>
If you want to run a configuration for more than 5 years, you need to use the `-n` option:

    payu run -n <number-of-runs>

This will run {{ model }} `number-of-runs` consecutive times, each with a *run length* equal to `restart_period`.<br>
This way, the *total experiment length* will be `restart_period * number-of-runs`. 

For example, to run a configuration for a total of 50 years with a `restart_period` of 5 years, the `number-of-runs` should be set to `10`:

    payu run -n 10

### Modify PBS resources

If the model has been altered and needs more time to complete, more memory, or needs to be submitted under a different NCI project, you will need to modify the following section in the `config.yaml`:

```yaml
# If submitting to a different project to your default, uncomment line below
# and replace PROJECT_CODE with appropriate code. This may require setting shortpath
# project: PROJECT_CODE

# Force payu to always find, and save, files in this scratch project directory
# shortpath: /scratch/PROJECT_CODE

queue: normal
walltime: 3:00:00
jobname: 1deg_jra55_ryf
mem: 1000GB
```

These lines can be edited to change the [PBS directives](https://opus.nci.org.au/display/Help/PBS+Directives+Explained) for the [PBS job][PBS job].

For example, to run {{ model }} under the `ol01` project (COSIMA Working Group), uncomment the line beginning with `# project` by deleting the `#` symbol and replace `PROJECT_CODE` wih `ol01`:

```yaml
project: ol01
```

!!! warning
    If projects other than `ol01` are used to run {{ model }} configuration, then the `shortpath` field also needs to be uncommented and the path to the desired `/scratch/PROJECT_CODE` added.<br>
    Doing this will make sure the same `/scratch` location is used for the _laboratory_, regardless of which project is used to run the experiment.
    <br><br>
    To run {{ model }}, you need to be a member of a project with allocated _Service Units (SU)_. For more information, check [how to join relevant NCI projects](/getting_started/set_up_nci_account#join-relevant-nci-projects).

### Syncing output data

The _laboratory_ directory is typically under the `/scratch` storage on _Gadi_, where [files are regularly deleted once they have been unaccessed for a period of time](https://opus.nci.org.au/pages/viewpage.action?pageId=156434436). For this reason climate model outputs need to be moved to a location with longer term storage.<br>
On _Gadi_, this is typically in a folder under a project code on `/g/data`.  

_Payu_ has built-in support to sync outputs, restarts and a copy of the _control_ directory git history to another location.<br>
This feature is controlled by the following section in the `config.yaml` file: 
```yaml
# Sync options for automatically copying data from ephemeral scratch space to 
# longer term storage
sync:
    enable: False # set path below and change to true
    path: none # Set to location on /g/data or a remote server and path (rsync syntax)
    exclude:
      - '*.nc.*'
      - 'iceh.????-??-??.nc'
```
To enable syncing, change `enable` to `True`, and set `path` to a location on `/g/data`, where _payu_ will copy output and restart folders. A sensible `path` could be: `/g/data/$PROJECT/$USER/{{model}}/experiment_name/`.

!!! admonition tip
    The {{model}} default configurations include a [userscript](#userscripts) in the _sync_ step that concatenates daily history/diagnostic output from the Sea-Ice model (CICE5) into monthly files. This speeds up access and saves storage space, but will only run if _sync_ is enabled.

### Saving model restarts

{{ model }} outputs restart files after every run to allow for subsequent runs to start from a previously saved model state.<br>
Restart files can occupy a significant amount of disk space, and keeping a lot of them is often not necessary.

The `restart_freq` field in the `config.yaml` file specifies a strategy for retaining restart files.<br>
This can either be a number (in which case every _nth_ restart file is retained), or one of the following pandas-style datetime frequencies:

- `YS` &rarr; start of the year
- `MS` &rarr; start of the month
- `D` &rarr; day
- `H` &rarr; hour
- `T` &rarr; minute
- `S` &rarr; second

For example, to preserve the ability to restart {{ model }} every 50 model-years, set:
```yaml
restart_freq: '50YS'
```

The most recent sequential restarts are retained, and only deleted after a permanently archived restart file has been produced.

For more information, check [_payu_ Configuration Settings documentation](https://payu.readthedocs.io/en/latest/config.html#model).

### Other configuration options

!!! warning
    The following sections in the `config.yaml` file control configuration options that are rarely modified, and often require a deeper understanding of how {{ model }} is structured to be safely changed.

#### Model configuration

This section tells _payu_ which driver to use for the main model configuration (`access-om2`) and the location of all inputs that are common to all its [model components].

```yaml
name: common
model: access-om2
input: /g/data/ik11/inputs/access-om2/input_20201102/common_1deg_jra55
```
The `name` field is not actually used for the configuration run, so it can be safely ignored.

#### Submodels

{{ model }} is a coupled model deploying multiple submodels (i.e. [model components]).

This section specifies the submodels and configuration options required to execute {{ model }} correctly.

Each submodel contains additional configuration options that are read in when the submodel is running. These options are specified in the subfolder of the _control_ directory whose name matches the submodel's `name` (e.g., configuration options for the `ocean` submodel are in the `~/access-om2/1deg_jra55_ryf/ocean` directory).

??? code "Expand to show the full `submodels` section"

    ```yaml
    submodels:
        - name: atmosphere
          model: yatm
          exe: /g/data/vk83/apps/spack/0.20/release/linux-rocky8-x86_64/intel-19.0.5.281/libaccessom2-git.2023.10.26=2023.10.26-ieiy3e7hidn4dzaqly3ly2yu45mecgq4/bin/yatm.exe
          input:
                - /g/data/vk83/experiments/inputs/access-om2/remapping_weights/JRA55/global.1deg/2020.05.30/rmp_jrar_to_cict_CONSERV.nc
                - /g/data/vk83/experiments/inputs/JRA-55/RYF/v1-4/data
          ncpus: 1

        - name: ocean
          model: mom
          exe: /g/data/vk83/apps/spack/0.20/release/linux-rocky8-x86_64/intel-19.0.5.281/mom5-git.2023.11.09=2023.11.09-ewcdbrfukblyjxpkhd3mfkj4yxfolal4/bin/fms_ACCESS-OM.x
          input:
            - /g/data/vk83/experiments/inputs/access-om2/ocean/grids/mosaic/global.1deg/2020.05.30/grid_spec.nc
            - /g/data/vk83/experiments/inputs/access-om2/ocean/grids/mosaic/global.1deg/2020.05.30/ocean_hgrid.nc
            - /g/data/vk83/experiments/inputs/access-om2/ocean/grids/mosaic/global.1deg/2020.05.30/ocean_mosaic.nc
            - /g/data/vk83/experiments/inputs/access-om2/ocean/grids/bathymetry/global.1deg/2020.10.22/topog.nc
            - /g/data/vk83/experiments/inputs/access-om2/ocean/grids/bathymetry/global.1deg/2020.10.22/ocean_mask.nc
            - /g/data/vk83/experiments/inputs/access-om2/ocean/grids/vertical/global.1deg/2020.10.22/ocean_vgrid.nc
            - /g/data/vk83/experiments/inputs/access-om2/ocean/processor_masks/global.1deg/216.16x15/2020.05.30/ocean_mask_table
            - /g/data/vk83/experiments/inputs/access-om2/ocean/chlorophyll/global.1deg/2020.05.30/chl.nc
            - /g/data/vk83/experiments/inputs/access-om2/ocean/initial_conditions/global.1deg/2020.10.22/ocean_temp_salt.res.nc
            - /g/data/vk83/experiments/inputs/access-om2/ocean/tides/global.1deg/2020.05.30/tideamp.nc
            - /g/data/vk83/experiments/inputs/access-om2/ocean/tides/global.1deg/2020.05.30/roughness_amp.nc
            - /g/data/vk83/experiments/inputs/access-om2/ocean/tides/global.1deg/2020.05.30/roughness_cdbot.nc
            - /g/data/vk83/experiments/inputs/access-om2/ocean/surface_salt_restoring/global.1deg/2020.05.30/salt_sfc_restore.nc
          ncpus: 216

        - name: ice
          model: cice5
          exe: /g/data/vk83/apps/spack/0.20/release/linux-rocky8-x86_64/intel-19.0.5.281/cice5-git.2023.10.19=2023.10.19-rh3xfkrgajya3ghtliacuhlx3pgvrzqs/bin/cice_auscom_360x300_24x1_24p.exe
          input:
            - /g/data/vk83/experiments/inputs/access-om2/ice/grids/global.1deg/2020.05.30/grid.nc
            - /g/data/vk83/experiments/inputs/access-om2/ice/grids/global.1deg/2020.10.22/kmt.nc
            - /g/data/vk83/experiments/inputs/access-om2/ice/initial_conditions/global.1deg/2020.05.30/i2o.nc
            - /g/data/vk83/experiments/inputs/access-om2/ice/initial_conditions/global.1deg/2020.05.30/o2i.nc
            - /g/data/vk83/experiments/inputs/access-om2/ice/initial_conditions/global.1deg/2020.05.30/u_star.nc
            - /g/data/vk83/experiments/inputs/access-om2/ice/initial_conditions/global.1deg/2020.05.30/monthly_sstsss.nc
          ncpus: 24
    ```

#### Collate

Rather than outputting a single diagnostic file over the whole model horizontal grid, [MOM](/models/model_components/ocean/#modular-ocean-model-mom) typically generates diagnostic outputs as tiles, each of which spans over a portion of the model horizontal grid.

The `collate` section in the `yaml.conf` file controls the process that combines these smaller files into a single output file.
```yaml
collate:
    restart: true
    walltime: 1:00:00
    mem: 30GB
    ncpus: 4
    queue: normal
    exe: /g/data/ik11/inputs/access-om2/bin/mppnccombine
```
Restart files are typically tiled in the same way and will also be combined together if the `restart` field is set to `true`.

#### Runlog

```yaml
runlog: true
```
When running a new configuration, _payu_ automatically commits changes with `git` if `runlog` is set to `true`.

!!! warning
    This should not be changed as it is an essential part of the provenance of an experiment.<br>
    _payu_ updates the manifest files for every run, and relies on `runlog` to save this information in the `git` history, so there is a record of all inputs, restarts, and executables used in an experiment.

#### Platform

```yaml
platform: 
    nodesize: 48
```
Set platform-specific default parameters.<br>
In the example above, the default number of cpus per node is set to 48. 
!!! warning
    This might need changing if the configuration is run on hardware with different node structure.

#### Userscripts

```yaml
userscripts:
    error: tools/resub.sh
    run: rm -f resubmit.count
    sync: /g/data/vk83/apps/om2-scripts/concatenate_ice/concat_ice_daily.sh 
```

A dictionary to run scripts or subcommands at various stages of a _payu_ submission.

- `error` gets called if the model does not run correctly and returns an error code.
- `run` gets called after the model execution, but prior to model output archive.
- `sync` gets called at the start of the sync pbs job. For more information refer to [Syncing output data](#syncing-output-data).
  
For more information about specific `userscripts` fields, check the relevant section of [_payu_ Configuration Settings documentation](https://payu.readthedocs.io/en/latest/config.html#postprocessing).

#### Miscellaneous

The following configuration settings should never require changing:

```yaml
stacksize: unlimited
mpirun: --mca io ompio --mca io_ompio_num_aggregators 1
qsub_flags: -W umask=027
env:
    UCX_LOG_LEVEL: 'error'
```

### Edit a single {{ model }} component configuration

Each of [{{ model }} components][model components] contains additional configuration options that are read in when the model component is running.<br>
These options are typically useful to modify the physics used in the model, the input data, or the model variables saved in the output files.

These configuration options are specified in files located inside a subfolder of the _control_ directory, named according to the submodel's `name` specified in the `config.yaml` `submodels` section (e.g., configuration options for the _ocean_ component are in the `~/access-om2/1deg_jra55_ryf/ocean` directory).<br>
To modify these options please refer to the User Guide of the respective model component.

### Create a custom {{ model }} build
All the executables needed to run {{ model }} are pre-built into independent configurations using _Spack_.<br>
To customise {{ model }}'s build (for example to run {{ model }} with changes in the source code of one of its component), refer to [Modify an ACCESS model's source code](/models/run-a-model/build_a_model#{{model|lower}}).

## Get Help

If you have questions or need help regarding {{ model }}, consider creating a topic in the [COSIMA category of the ACCESS-Hive Forum](https://forum.access-hive.org.au/c/cosima/29).<br>
For assistance on how to request help from ACCESS-NRI, follow the [guidelines on how to get help](/about/user_support/#still-need-help).

<custom-references>
- [https://cosima.org.au](https://cosima.org.au)
- [Kiss et al. (2020)](http://doi.org/10.5194/gmd-13-401-2020)
- [https://payu.readthedocs.io/en/latest/usage.html](https://payu.readthedocs.io/en/latest/usage.html)
- [https://github.com/access-nri/access-om2](https://github.com/access-nri/access-om2)
- [https://opus.nci.org.au/](https://opus.nci.org.au/)
</custom-references>
