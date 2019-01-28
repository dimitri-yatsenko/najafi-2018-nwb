# najafi-2018-nwb

This project presents the data accompanying the paper
> Najafi, Farzaneh, Gamaleldin F. Elsayed, Eftychios Pnevmatikakis, John Cunningham, and Anne K. Churchland. "Inhibitory and excitatory populations in parietal cortex are equally selective for decision outcome in both novices and experts." bioRxiv (2018): 354340.

https://www.biorxiv.org/content/early/2018/10/10/354340

The original data are available from Cold Spring Harbor Laboratory:  http://repository.cshl.edu/36980/

# Converting the original data
The data download instructions are for a Unix-family OS such as Linux or Mac OS with Python 3.7+ on the system path as `python3`. 

## Clone this repository and download the data
In the terminal window, git clone

```console
$ git clone https://github.com/vathes/najafi-2018-nwb.git
$ cd najafi-2018-nwb
``` 

## Download the original data 

The following command will download the original data from CSHL (~70 TB).
```console 
$ mkdir data
$ python3 scripts/download.py
```
This may take several hours.  If the download is interrupted, simply re-run `download.py` and it will pick up where it left.

Verify that all 18 files have downloaded.
```console
$ ls data
FN_dataSharing.tgz-aa	FN_dataSharing.tgz-af	FN_dataSharing.tgz-ak	FN_dataSharing.tgz-ap
FN_dataSharing.tgz-ab	FN_dataSharing.tgz-ag	FN_dataSharing.tgz-al	FN_dataSharing.tgz-aq
FN_dataSharing.tgz-ac	FN_dataSharing.tgz-ah	FN_dataSharing.tgz-am	FN_dataSharing.tgz-ar
FN_dataSharing.tgz-ad	FN_dataSharing.tgz-ai	FN_dataSharing.tgz-an
FN_dataSharing.tgz-ae	FN_dataSharing.tgz-aj	FN_dataSharing.tgz-ao
```

Now unpack the tar files:

```console
$ cat data/FN_dataSharing.tgz-a* | tar -C data -xzf -
```

Verify that the data have unpacked:

```console
$ ls data/FN_dataSharing
bag-info.txt		data			manifest-sha256.txt	tagmanifest-sha256.txt
bagit.txt		manifest-md5.txt	tagmanifest-md5.txt
```


## Conversion to NWB 2.0
The following command will convert the dataset into the NWB 2.0 format (See https://neurodatawithoutborders.github.io/)

```console
$ python3 scripts/NWB_convert.py
```

The resulted data directory includes a `manifest.txt` file specifying all available data, and a data folder containing the `.mat` files.

The conversion to NWB 2.0 format is done via the [`NWB_conversion.py`](https://github.com/ttngu207/najafi-2018-nwb/blob/master/scripts/NWB_conversion.py) script. This script takes one argument, a `.json` config file, specifying the *manifest* file, output directory, and some metadata. 

An example content of the *.json* config file is as follow: 
```json
{
	"manifest": "data/manifest-md5.txt",
	"general": 
		{
			"experimenter" : "Farzaneh Najafi",
			"institution" : "Cold Spring Harbor Laboratory",
			"related_publications" : "https://doi.org/10.1101/354340"
		},
	"error_log" : "data/conversion_error_log.txt",
	"output_dir" : "data/NWB 2.0"
}
```

The converted NWB 2.0 files will be saved in the `output_dir` directory specified in the *.json* file. Running the conversion script is as follow: 

```console
$ python3 NWB_conversion conversion_config.json
```

# Showcase work with NWB:N files
This repository will contain Jupyter Notebook demonstrating how to navigate and query the dataset. 

See this [Jupyter Notebook](https://github.com/ttngu207/najafi-2018-nwb/blob/master/notebooks/NWB2.0_Tutorial.ipynb) for a tutorial on using [**PyNWB**](https://pynwb.readthedocs.io/en/latest/) API to access NWB 2.0 data, to process and plot some of the key figures presented in this study (https://doi.org/10.1101/354340). 
