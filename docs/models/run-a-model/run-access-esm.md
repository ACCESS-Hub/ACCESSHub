{% set model = "ACCESS-ESM1.5" %}
{% set github_configs = "https://github.com/ACCESS-NRI/access-esm1.5-configs" %}
[PBS job]: https://opus.nci.org.au/display/Help/4.+PBS+Jobs
[gadi]: https://opus.nci.org.au/display/Help/0.+Welcome+to+Gadi#id-0.WelcometoGadi-Overview
[payu]: https://github.com/payu-org/payu
[model components]: /models/configurations/access-esm/#model-components
[model configurations]: /models/configurations/access-esm/#access-esm15

[:fontawesome-brands-github:{: class="twemoji icon-before-text"} {{ model }} configurations]({{github_configs}}){: class="text-card"}

# Run {{ model }}

## About

{{ model }} is a fully-coupled global climate model, combining  atmosphere, land, ocean, sea ice, ocean biogeochemistry and land biogeochemistry components. A description of the model and its components is available in the [{{ model }} overview][model configurations].

The instructions below outline how to run {{ model }} using ACCESS-NRI's software deployment pipeline, specifically designed to run on the [National Computating Infrastructure (NCI)](https://nci.org.au/about-us/who-we-are) supercomputer [_Gadi_][gadi].

If you are unsure whether {{ model }} is the right choice for your experiment, take a look at the overview of [ACCESS Models](/models).

All {{model}} configurations are open source, licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/?ref=chooser-v1")![CC icon](https://mirrors.creativecommons.org/presskit/icons/cc.svg?ref=chooser-v1){: style="height:1em;margin-left:0.2em;vertical-align:text-top;"}![BY icon](https://mirrors.creativecommons.org/presskit/icons/by.svg?ref=chooser-v1){: style="height:1em;margin-left:0.2em;vertical-align:text-top;"} and available on [ACCESS-NRI GitHub]({{github_configs}}).

{{ model }} release notes are [available on the ACCESS-Hive Forum](https://forum.access-hive.org.au/t/access-esm1-5-release-information/2352) and are updated when new releases are made available.

## Prerequisites

### General prerequisites

Before running {{ model }}, you need to [Set Up your NCI Account](/getting_started/set_up_nci_account).

### Model-specific prerequisites

- **_MOSRS_ account**<br>
    The [Met Office Science Repository Service (MOSRS)](https://code.metoffice.gov.uk) is a server run by the UK Met Office (UKMO) to support collaborative development with other partners organisations. MOSRS contains the source code and configurations for some model components in {{ model }} (e.g., the [UM](/models/model_components/atmosphere/#unified-model-um)).<br>
    To apply for a _MOSRS_ account, please contact your [local institutional sponsor](https://opus.nci.org.au/display/DAE/Prerequisites).

- **Join the _vk83_, _ki32_ and _ki32\_mosrs_ projects at NCI**<br>
    To join these projects, request membership on the respective [vk83](https://my.nci.org.au/mancini/project/vk83/join), [ki32](https://my.nci.org.au/mancini/project/ki32/join) and [ki32_mosrs](https://my.nci.org.au/mancini/project/ki32_mosrs/join) NCI project pages.

    !!! tip
        To request membership for the _ki32_mosrs_ subproject you need to be member of the _ki32_ project first.

    For more information on joining specific NCI projects, refer to [How to connect to a project](https://opus.nci.org.au/display/Help/How+to+connect+to+a+project).

- **Payu**<br>
    [_Payu_][payu] is a workflow management tool for running numerical models in supercomputing environments, for which there is extensive [documentation](https://payu.readthedocs.io/en/latest/).<br>
    _Payu_ on _Gadi_ is available through a dedicated `conda` environment in the _vk83_ project.<br>
    After joining the _vk83_ project, load the `payu` module:
    
        module use /g/data/vk83/modules
        module load payu

    To check that _payu_ is available, run:

        payu --version
    
    <terminal-window>
        <terminal-line data="input">payu --version</terminal-line>
        <terminal-line lineDelay="1000">1.1.5</terminal-line>
    </terminal-window>

    !!! warning
        _payu_ version >=1.1.5 is required

----------------------------------------------------------------------------------------

## Get {{ model }} configuration

All released {{ model }} configurations are available from the [{{ model }} configs]({{github_configs}}) GitHub repository.<br>
Released configurations are tested and supported by ACCESS-NRI, as an adaptation of those originally developed by [CSIRO](https://www.csiro.au/en/research/environmental-impacts/climate-change/climate-science-centre) and [CLEX CMS](https://github.com/coecms/access-esm).

For more information on {{ model }} configurations, check [{{model}}][model configurations] page.

More information about the available experiments and the naming scheme of the branches can also be found in the [{{ model }} configs]({{github_configs}}) GitHub repository.

The first step is to choose a configuration from those available.<br>

For example, if the required configuration is the co2 concentration driven pre-industrial configuration, then the branch to select is [`release-preindustrial+concentrations`](https://forum.access-hive.org.au/t/access-esm1-5-release-information/2352).

To clone this branch to a location on _Gadi_, run:
    
    mkdir -p ~/access-esm1.5
    cd ~/access-esm1.5
    payu clone -b expt -B release-preindustrial+concentrations {{ github_configs }} preindustrial+concentrations

In the example above the `payu clone` command clones the concentration driven pre-industrial configuration (`-B release-preindustrial+concentrations`) 
to a new experiment branch (`-b expt`) to a directory named `preindustrial+concentrations`.
!!! admonition tip
    Anyone using a configuration is advised to clone only a single branch (as shown in the example above) and not the entire repository.

<terminal-window>
    <terminal-line data="input">mkdir -p ~/access-esm1.5</terminal-line>
    <terminal-line data="input">cd ~/access-esm1.5</terminal-line>
    <terminal-line data="input" directory="~/access-esm1.5">payu clone -b expt -B release-preindustrial+concentrations {{ github_configs }} preindustrial+concentrations</terminal-line>
    <terminal-line lineDelay=1000>Cloned repository from {{ github_configs }} to directory: .../access-esm1.5/preindustrial+concentrations</terminal-line>
    <terminal-line>Created and checked out new branch: expt</terminal-line>
    <terminal-line>laboratory path:   /scratch/.../access-esm</terminal-line>
    <terminal-line>binary path:  /scratch/.../access-esm/bin</terminal-line>
    <terminal-line>input path:  /scratch/.../access-esm/input</terminal-line>
    <terminal-line>work path:  /scratch/.../access-esm/work</terminal-line>
    <terminal-line>archive path:  /scratch/.../access-esm/archive</terminal-line>
    <terminal-line>Updated metadata. Experiment UUID: 0635396b-678d-45f9-b81e-abd25a2d7bf0</terminal-line>
    <terminal-line>Added archive symlink to /scratch/.../access-esm/archive/preindustrial+concentrations-expt-0635396b</terminal-line>
    <terminal-line>To change directory to control directory run:</terminal-line>
    <terminal-line data="input">cd preindustrial+concentrations</terminal-line>
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

- The _control_ directory contains the model configuration and serves as the execution directory for running the model (in this example, the cloned directory `~/access-esm1.5/preindustrial+concentrations`).
- The _laboratory_ directory, where all the model components reside. For {{ model }}, it is typically `/scratch/$PROJECT/$USER/access-esm`.

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

This will submit a single job to the queue with a run length given by [`runtime`](#runtime) in the `config.yaml` file.<br>

<terminal-window>
    <terminal-line data="input">cd ~/access-esm1.5/preindustrial+concentrations</terminal-line>
    <terminal-line directory="~/access-esm1.5/preindustrial+concentrations" data="input">payu run</terminal-line>
    <terminal-line lineDelay=50>Loading input manifest: manifests/input.yaml</terminal-line>
    <terminal-line lineDelay=50>Loading restart manifest: manifests/restart.yaml</terminal-line>
    <terminal-line lineDelay=50>Loading exe manifest: manifests/exe.yaml</terminal-line>
    <terminal-line lineDelay=50>payu: Found modules in /opt/Modules/v4.3.0</terminal-line>
    <terminal-line lineDelay=50>
        qsub -q normal -P tm70 -l walltime=9000 -l ncpus=384 -l mem=1536GB -N pre-industrial -l wd -j n -v PAYU_PATH=/g/data/vk83/apps/payu/1.1.4/bin,MODULESHOME=/opt/Modules/v4.3.0,MODULES_CMD=/opt/Modules/v4.3.0/libexec/modulecmd.tcl,MODULEPATH=/g/data/vk83/modules:/etc/scl/modulefiles:/apps/Modules/restricted-modulefiles/matlab_monash:/opt/Modules/modulefiles:/opt/Modules/v4.3.0/modulefiles:/apps/Modules/modulefiles -W umask=027 -l storage=gdata/vk83 -- /g/data/vk83/apps/payu/1.1.4/bin/python3.10 /g/data/vk83/apps/payu/1.1.4/bin/payu-run
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
    <terminal-line linedelay=0>&lt;job-ID&gt;.gadi-pbs&nbsp;&nbsp;pre-industrial&nbsp;&nbsp;&nbsp;&lt;$USER&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;time&gt;&nbsp;R&nbsp;normal-exec</terminal-line>
</terminal-window>

To show the status of all your submitted [PBS jobs][PBS job], you can execute the following command:
```
qstat -u $USER
```

<terminal-window>
    <terminal-line data="input">qstat -u $USER</terminal-line>
    <terminal-line linedelay=500>Job id&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Name&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;User&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Time Use&nbsp;S Queue</terminal-line>
    <terminal-line linedelay=0>---------------------  ---------------- ----------------  -------- - -----</terminal-line>
    <terminal-line linedelay=0>&lt;job-ID&gt;.gadi-pbs&nbsp;&nbsp;&nbsp;pre-industrial&nbsp;&nbsp;&nbsp;&lt;$USER&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;time&gt;&nbsp;R&nbsp;normal-exec</terminal-line>
    <terminal-line linedelay=0>&lt;job-ID&gt;.gadi-pbs&nbsp;&nbsp;&nbsp;&lt;other-job-name&gt;&nbsp;&lt;$USER&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;time&gt;&nbsp;R&nbsp;normal-exec</terminal-line>
    <terminal-line linedelay=0>&lt;job-ID&gt;.gadi-pbs&nbsp;&nbsp;&nbsp;&lt;other-job-name&gt;&nbsp;&lt;$USER&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;time&gt;&nbsp;R&nbsp;normal-exec</terminal-line>
</terminal-window>

The default name of your job is the name of the _payu_ _control_ directory (`preindustrial+concentrations` in the example above).<br>
This can be overwritten by altering the `jobname` in the [PBS resources section](#modify-pbs-resources) of the `config.yaml` file.

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
    If you started an {{ model }} run using the `-n` option (e.g., to [run the model for several years](#multiple-runs)), but subsequently decide not to keep running after the current process completes, you can create a file called `stop_run` in the _control_ directory.<br>
    This will prevent _payu_ from submitting another job.

### Error and output log files

#### PBS output files
When the model completes a run, PBS writes the standard output and error streams to two files inside the _control_ directory: `<jobname>.o<job-ID>` and `<jobname>.e<job-ID>`, respectively.

These files usually contain logs about _payu_ tasks, and give an overview of the resources used by the job.<br>
To move these files to the `archive` directory, use the following command:
```
payu sweep
```

#### Model log files

While the model is running, _payu_ saves the model standard output and error streams in the `access.out` and `access.err` files inside the _control_ directory, respectively.<br>
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
    <terminal-line>laboratory path: /scratch/$PROJECT/$USER/access-esm</terminal-line>
    <terminal-line>binary path: /scratch/$PROJECT/$USER/access-esm/bin</terminal-line>
    <terminal-line>input path: /scratch/$PROJECT/$USER/access-esm/input</terminal-line>
    <terminal-line>work path: /scratch/$PROJECT/$USER/access-esm/work</terminal-line>
    <terminal-line>archive path: /scratch/$PROJECT/$USER/access-esm/archive</terminal-line>
    <terminal-line>Found experiment archive: /scratch/$PROJECT/$USER/access-esm/archive/preindustrial+concentrations-expt-0635396b</terminal-line>
    <terminal-line>payu: Found modules in /opt/Modules/v4.3.0</terminal-line>
    <terminal-line>Loading access-esm1p5/2024.05.0</terminal-line>
    <terminal-line>    Loading requirement: cice4/2024.05.21 mom5/access-esm1.5_2024.06.20 um7/2024.07.03</terminal-line>
    <terminal-line>Loading input manifest: manifests/input.yaml</terminal-line>
    <terminal-line>Loading restart manifest: manifests/restart.yaml</terminal-line>
    <terminal-line>Loading exe manifest: manifests/exe.yaml</terminal-line>
    <terminal-line>Making exe links</terminal-line>
    <terminal-line>Setting up atmosphere</terminal-line>
    <terminal-line>Setting up ocean</terminal-line>
    <terminal-line>Setting up ice</terminal-line>
    <terminal-line>Setting up coupler</terminal-line>
    <terminal-line>Checking exe and input manifests</terminal-line>
    <terminal-line>Creating restart manifest</terminal-line>
    <terminal-line>Updating full hashes for 29 files in manifests/restart.yaml</terminal-line>
    <terminal-line>Writing manifests/restart.yaml</terminal-line>
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
    <terminal-line data="input">cd ~/access-esm/preindustrial+concentrations/archive</terminal-line>
    <terminal-line data="input" directory="~/access-esm/preindustrial+concentrations/archive">ls</terminal-line>
    <terminal-line class="ls-output-format">metadata.yaml output000 pbs_logs restart000</terminal-line>
</terminal-window>

----------------------------------------------------------------------------------------

## Edit {{ model }} configuration

This section describes how to modify {{ model }} configuration.<br>
The modifications discussed in this section can change the way {{ model }} is run by _payu_, or how its specific [model components] are configured and coupled together.

The `config.yaml` file located in the _control_ directory is the _Master Configuration_ file, which controls the general model configuration. It contains several parts, some of which it is more likely will need modification, and others which are rarely changed without having a deep understanding of how the model is configured.

To find out more about configuration settings for the `config.yaml` file, refer to [how to configure your experiment with payu](https://payu.readthedocs.io/en/latest/config.html).

### Run lengths {: id="runtime"}

One of the most common changes is to adjust the duration of the model run.<br> {{model}} simulations are split into smaller _run lengths_, each with the duration specified by the `runtime` settings in the `config.yaml` file:

The length of an {{model}} run is controlled by the `runtime` settings in the `config.yaml` file:

```yml
    runtime:
        years: 1
        months: 0
        days: 0
```
At the end of each run length, each model component saves its state into a _restart file_, allowing the simulation to be continued in subsequent runs.

!!! warning
    The _run length_ (controlled by `runtime`) should be left at 1 year for {{model}} experiments in order to avoid errors. Shorter simulations can be useful when setting up and debugging new experiments, however they require additional configuration changes. See the section [Running for less than one year](#shorter-runs) for details.

To run {{ model }} configuration for multiple subsequent _run lengths_ (each with duration `runtime` in the `config.yaml` file), use the option `-n` with the `payu run` command:

```
payu run -f -n <number-of-runs>
```

This will run the configuration `number-of-runs` times, resulting in a _total experiment length_ of `runtime * number-of-runs`. The runs will be split across a number of consecutive [PBS jobs][PBS job] submitted to the queue, as controlled by the `runspersub` value specified in the config.yaml file.
    
### Understand `runtime`, `runspersub`, and `-n` parameters {: id="multiple-runs"}

It is possible to have more than one model run per queue submit. With the correct use of [`runtime`](#runtime), `runspersub` and `-n` parameters, you can have full control of your experiment.<br>

- `runtime` defines the _run length_.
- `runspersub` defines the maximum number of runs for every [PBS job] submission.
- `walltime` defines the maximum time of every [PBS job] submission.
- `-n` sets the number of runs to be performed.

Now some practical examples:

- **Run 20 years of simulation with resubmission every 5 years**<br>
    To have a _total experiment length_ of 20 years with a 5-year resubmission cycle, leave [`runtime`](#runtime) as the default value of `1 year`, set `runspersub` to `5` and `walltime` to `10:00:00`. Then, run the configuration with `-n` set to `20`:
    ```
    payu run-f -n 20
    ```
    This will submit subsequent jobs for the following years: 1 to 5, 6 to 10, 11 to 15, and 16 to 20, which is a total of 4 PBS jobs.

- **Run 7 years of simulation with resubmission every 3 years**<br>
    To have a _total experiment length_ of 7 years with a 3-year resubmission cycle, leave [`runtime`](#runtime) as the default value of `1 year`, set `runspersub` to `3` and `walltime` to `6:00:00`. Then, run the configuration with `-n` set to `7`:
    ```
    payu run -f -n 7
    ```
    This will submit subsequent jobs for the following years: 1 to 3, 4 to 6, and 7, which is a total of 3 PBS jobs.
!!! tip
    The `walltime` must be set to be long enough that the PBS job can complete. The model usually runs a single year in 90 minutes or less, but the `walltime` for a single model run is set to `2:30:00` out of an abundance of caution to make sure the model has time to run when there are occasional slower runs for unpredictable reasons. When setting `runspersub > 1` the `walltime` doesn't need to be a simple multiple of `2:30:00` because it is highly unlikely that there will be multiple anomalously slow runs per submit.
### Running for less than one year {: id="shorter-runs"}
When debugging changes to a model, it is common to reduce the run length to minimise resource consumption and return faster feedback on changes. In order to run the model for a single month, the `runtime` can be changed to

```yml
    runtime:
        years: 0
        months: 1
        days: 0
```

With the default configuration settings, the sea ice component of {{ model }} will produce restart files only at the end of each year. If valid restart files are required when running shorter simulations, the sea ice model configuration should be modified so that restart files are produced at monthly frequencies. To do this, change the `dumpfreq = 'y'` setting to `dumpfreq = 'm'` in the `cice_in.nml` configuration file located in the `ice` subdirectory of the _control_ directory.

### Modify PBS resources

If the model has been altered and needs more time to complete, more memory, or needs to be submitted under a different NCI project, you will need to modify the following section in the `config.yaml`:


```yaml
# PBS configuration

# If submitting to a different project to your default, uncomment line below
# and replace PROJECT_CODE with appropriate code. This may require setting shortpath
# project: PROJECT_CODE

# Force payu to always find, and save, files in this scratch project directory
# shortpath: /scratch/PROJECT_CODE

# Note: if laboratory is relative path, it is relative to shortpath/$USER
laboratory: access-esm

jobname: pre-industrial
queue: normal
walltime: 2:30:00
```

These lines can be edited to change the [PBS directives](https://opus.nci.org.au/display/Help/PBS+Directives+Explained) for the [PBS job][PBS job].

By default the model will be submitted to the PBS queue using your default project.  To run {{ model }} using the resources of a specific project, for example the `lg87` project (ESM Working Group), uncomment the line beginning with `# project` by deleting the `#` symbol and replace `PROJECT_CODE` with `lg87`:

```yaml
project: lg87
```

!!! warning
    If more than one project is used to run an {{ model }} configuration the `shortpath` option also needs to be uncommented and the path to the desired `/scratch/PROJECT_CODE` directory added.<br>
    This ensures the same `/scratch` location is used for the _laboratory_, regardless of which project is used to run the experiment.
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
    path: null # Set to location on /g/data or a remote server and path (rsync syntax)
```
To enable syncing, change `enable` to `True`, and set `path` to a location on `/g/data`, where _payu_ will copy output and restart folders.

!!! Warning
    The {{model}} configurations include a [postprocessing script](#postscripts) which converts atmospheric outputs to NetCDF format. This script runs in a separate PBS job and prevents the output and restart files of the most recent run from being automatically synced.<br>
    After a series of runs and the final post-processing is completed, manually execute `payu sync` in the _control_ directory to sync the final output and restart files.

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

This section tells _payu_ which driver to use for the main model (`access` refers to {{ model }}).  

```yaml
model: access
```


#### Submodels

{{ model }} is a coupled model deploying multiple submodels (i.e. [model components]).

This section specifies the submodels and configuration options required to execute {{ model }} correctly.

Each submodel contains additional configuration options that are read in when the submodel is running. These options are specified in the subfolder of the _control_ directory, whose name matches the submodel's `name` (e.g., configuration options for the `atmosphere` submodel are in the `~/access-esm/preindustrial+concentrations/atmosphere` directory).


??? code "Expand to show the full `submodels` section"

    ```yaml
    submodels:
    - name: atmosphere
      model: um
      ncpus: 192
      exe: um_hg3.exe
      input:
        # Aerosols
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/pre-industrial/atmosphere/aerosol/global.N96/2020.05.19/OCFF_1850_ESM1.anc
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/pre-industrial/atmosphere/aerosol/global.N96/2020.05.19/BC_hi_1850_ESM1.anc
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/pre-industrial/atmosphere/aerosol/global.N96/2020.05.19/scycl_1850_ESM1_v4.anc
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/pre-industrial/atmosphere/aerosol/global.N96/2020.05.19/Bio_1850_ESM1.anc
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/share/atmosphere/aerosol/global.N96/2020.05.19/biogenic_351sm.N96L38
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/share/atmosphere/aerosol/global.N96/2020.05.19/sulpc_oxidants_N96_L38
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/share/atmosphere/aerosol/global.N96/2020.05.19/DMS_conc.N96
        # Forcing
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/pre-industrial/atmosphere/forcing/global.N96/2020.05.19/ozone_1850_ESM1.anc
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/pre-industrial/atmosphere/forcing/resolution_independent/2020.05.19/volcts_18502000ave.dat
        # Land
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/pre-industrial/atmosphere/land/biogeochemistry/global.N96/2020.05.19/Ndep_1850_ESM1.anc
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/share/atmosphere/land/soiltype/global.N96/2020.05.19/qrparm.soil_igbp_vg
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/share/atmosphere/land/vegetation/global.N96/2020.05.19/cable_vegfunc_N96.anc
        - /g/data/vk83/configurations/inputs/access-esm1p5/share/atmosphere/land/biogeochemistry/resolution_independent/2020.05.19/modis_phenology_csiro.txt
        - /g/data/vk83/configurations/inputs/access-esm1p5/share/atmosphere/land/biogeochemistry/resolution_independent/2020.05.19/pftlookup_csiro_v16_17tiles_wtlnds.csv
        - /g/data/vk83/configurations/inputs/access-esm1p5/share/atmosphere/land/biogeophysics/resolution_independent/2020.05.19/def_soil_params.txt
        - /g/data/vk83/configurations/inputs/access-esm1p5/share/atmosphere/land/biogeophysics/resolution_independent/2020.05.19/def_veg_params.txt
        # Spectral
        - /g/data/vk83/configurations/inputs/access-esm1p5/share/atmosphere/spectral/resolution_independent/2020.05.19/spec3a_sw_hadgem1_6on
        - /g/data/vk83/configurations/inputs/access-esm1p5/share/atmosphere/spectral/resolution_independent/2020.05.19/spec3a_lw_hadgem1_6on
        # Grids
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/share/atmosphere/grids/global.N96/2020.05.19/qrparm.mask
        - /g/data/vk83/configurations/inputs/access-esm1p5/share/atmosphere/grids/resolution_independent/2020.05.19/vertlevs_G3
        # STASH
        - /g/data/vk83/configurations/inputs/access-esm1p5/share/atmosphere/stash/2020.05.19/

    - name: ocean
      model: mom
      ncpus: 180
      exe: fms_ACCESS-CM.x
      input:
        # Biogeochemistry
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/share/ocean/biogeochemistry/global.1deg/2020.05.19/dust.nc
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/share/ocean/biogeochemistry/global.1deg/2020.05.19/ocmip2_press_monthly_om1p5_bc.nc
        - /g/data/vk83/configurations/inputs/access-esm1p5/share/ocean/biogeochemistry/global.1deg/2024.07.12/bgc_param.nc
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/unused/ocean/biogeochemistry/global.1deg/2020.05.19/ocmip2_fice_monthly_om1p5_bc.nc
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/unused/ocean/biogeochemistry/global.1deg/2020.05.19/ocmip2_xkw_monthly_om1p5_bc.nc
        # Tides
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/share/ocean/tides/global.1deg/2020.05.19/roughness_amp.nc
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/share/ocean/tides/global.1deg/2020.05.19/tideamp.nc
        # Shortwave
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/share/ocean/shortwave_penetration/global.1deg/2020.05.19/ssw_atten_depth.nc
        # Grids
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/share/ocean/grids/mosaic/global.1deg/2020.05.19/grid_spec.nc

    - name: ice
      model: cice
      ncpus: 12
      exe: cice_access_360x300_12x1_12p.exe
      input:
        # Grids
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/share/ice/grids/global.1deg/2020.05.19/kmt.nc
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/share/ice/grids/global.1deg/2020.05.19/grid.nc
        # Climatology
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/share/ice/climatology/global.1deg/2020.05.19/monthly_sstsss.nc

    - name: coupler
      model: oasis
      ncpus: 0
      input:
        # Grids
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/share/coupler/grids/global.oi_1deg.a_N96/2020.05.19/grids.nc
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/share/coupler/grids/global.oi_1deg.a_N96/2020.05.19/areas.nc
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/share/coupler/grids/global.oi_1deg.a_N96/2020.05.19/masks.nc
        # Remapping weights
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/share/coupler/remapping_weights/global.oi_1deg.a_N96/2020.05.19/rmp_cice_to_um1t_CONSERV_FRACNNEI.nc
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/share/coupler/remapping_weights/global.oi_1deg.a_N96/2020.05.19/rmp_um1u_to_cice_CONSERV_FRACNNEI.nc
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/share/coupler/remapping_weights/global.oi_1deg.a_N96/2020.05.19/rmp_um1t_to_cice_CONSERV_DESTAREA.nc
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/share/coupler/remapping_weights/global.oi_1deg.a_N96/2020.05.19/rmp_cice_to_um1u_CONSERV_FRACNNEI.nc
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/share/coupler/remapping_weights/global.oi_1deg.a_N96/2020.05.19/rmp_um1v_to_cice_CONSERV_FRACNNEI.nc
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/share/coupler/remapping_weights/global.oi_1deg.a_N96/2020.05.19/rmp_um1t_to_cice_CONSERV_FRACNNEI.nc
        - /g/data/vk83/configurations/inputs/access-esm1p5/modern/share/coupler/remapping_weights/global.oi_1deg.a_N96/2020.05.19/rmp_cice_to_um1v_CONSERV_FRACNNEI.nc

    ```

#### Collate

Rather than outputting a single diagnostic file over the whole model horizontal grid, the ocean component [MOM](/models/model_components/ocean/#modular-ocean-model-mom) typically generates diagnostic outputs as tiles, each of which spans a portion of model grid.

The `collate` section in the `config.yaml` file controls the process that combines these smaller files into a single output file.

```yaml
# Collation
collate:
    exe: mppnccombine.spack
    restart: true
    mem: 4GB
    walltime: 1:00:00
    mpi: false
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


#### Userscripts
```yaml
userscripts:
    # Apply land use changes after each run
    run: ./scripts/update_landuse_driver.sh
```

Run scripts or subcommands at various stages of a _payu_ submission. The above example comes from the `release-historical+concentrations` configuration, where the ```update_landuse_driver.sh``` is used to apply historical land use changes at the end of each run.

For more information about specific `userscripts` fields, check the relevant section of [_payu_ Configuration Settings documentation](https://payu.readthedocs.io/en/latest/config.html#postprocessing).


#### Postscripts
Postprocessing scripts that run after _payu_ has completed all steps. Scripts that might alter the output directory, for example, can be run as postscripts. These run in PBS jobs separate from the main model simulation.

```yaml
postscript: -v PAYU_CURRENT_OUTPUT_DIR,PROJECT -lstorage=${PBS_NCI_STORAGE} ./scripts/NetCDF-conversion/UM_conversion_job.sh
```

All {{ model }} configurations include the NetCDF conversion postscript mentioned above. This script converts the [UM](/models/model_components/atmosphere#unified-model-um)'s fields file format output to NetCDF in order to facilitate analysis and reduce storage requirements. By default, the conversion script will delete the fields files upon successful completion, leaving only the NetCDF output. This automatic deletion can be disabled by commenting out the `--delete-ff` command line flag from the conversion job submission script located in the _control_ directory under `scripts/NetCDF-conversion/UM_conversion_job.sh`.<br>
That means changing

```bash
esm1p5_convert_nc $PAYU_CURRENT_OUTPUT_DIR --delete-ff
```

to

```bash
esm1p5_convert_nc $PAYU_CURRENT_OUTPUT_DIR # --delete-ff
```

#### Miscellaneous

The following configuration settings should never require changing:

```yaml
stacksize: unlimited
qsub_flags: -W umask=027
```

### Edit a single {{ model }} component configuration

Each of [{{ model }} components][model components] contains additional configuration options that are read in when the model component is running.<br> These options are typically useful to modify the physics used in the model, the input data, or the model variables saved in the output files.

These configuration options are specified in files located inside a subfolder of the _control_ directory, named according to the submodel's `name` specified in the `config.yaml` `submodels` section (e.g., configuration options for the _ocean_ component are in the `~/access-esm/preindustrial+concentrations/ocean` directory).<br>
To modify these options please refer to the User Guide of the respective model component.

### Create a custom {{ model }} build
All the executables needed to run {{ model }} are pre-built into independent configurations using _Spack_.<br>
To customise {{ model }}'s build (for example to run {{ model }} with changes in the source code of one of its component), refer to [Modify an ACCESS model's source code](/models/run-a-model/build_a_model#{{model|lower}}).

### Controlling model output
Selecting the variables to save from a simulation can be a balance between enabling future analysis and minimising storage requirements. The choice and frequency of variables saved by each model can be configured from within each submodel's _control_ directory. 

Each submodel's _control_ directory contains _detailed_ and _standard_ presets for controlling the output, located in the `diagnostic_profiles` subdirectories (e.g. `~/access-esm/preindustrial+concentrations/ice/diagnostic_profiles` for the sea ice submodel). The _detailed_ profiles request a large number of variables at higher frequencies, while the _standard_ profiles restrict the output to variables more regularly used across the community. Details on the variables saved by each preset are available in [this Hive Forum topic](https://forum.access-hive.org.au/t/preset-output-profiles-for-esm1-5/3629).

Selecting a preset output profile to use in a simulation can be done by pointing the following symbolic links to the desired profile:

 * `STASHC` in the atmosphere _control_ directory.
 * `diag_table` in the ocean _control_ directory.
 * `ice_history.nml` in the ice _control_ directory.

For example, to select the _detailed_ output profile for the atmosphere:
<terminal-window>
    <terminal-line data="input">cd ~/access-esm/preindustrial+concentrations/atmosphere</terminal-line>
    <terminal-line data="input">ln -sf diagnostic_profiles/STASHC_detailed STASHC</terminal-line>
</terminal-window>


## Get Help

If you have questions or need help regarding {{ model }}, consider creating a topic in the [Earth System Model category of the ACCESS-Hive Forum](https://forum.access-hive.org.au/c/esm/earth-system-model/72).<br>
For assistance on how to request help from ACCESS-NRI, follow the [guidelines on how to get help](/about/user_support/#still-need-help).

<custom-references>
- [https://github.com/access-nri/access-esm1.5](https://github.com/access-nri/access-esm1.5)
- [https://opus.nci.org.au/](https://opus.nci.org.au/)
- [https://github.com/coecms/esm-pre-industrial](https://github.com/coecms/esm-pre-industrial)
- [https://payu.readthedocs.io/en/latest/usage.html](https://payu.readthedocs.io/en/latest/usage.html)
</custom-references>
