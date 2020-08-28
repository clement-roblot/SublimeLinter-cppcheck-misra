# SublimeLinter-cppcheck-misra

[![Build Status](https://travis-ci.org/ChisholmKyle/SublimeLinter-cppcheck-misra.svg?branch=master)](https://travis-ci.org/ChisholmKyle/SublimeLinter-cppcheck-misra)

This linter plugin for [SublimeLinter](https://github.com/SublimeLinter/SublimeLinter) provides an interface to [cppcheck](https://github.com/danmar/cppcheck) with the development-branch [misra.py addon](https://github.com/danmar/cppcheck/tree/master/addons). It will be used with files that have the “c” syntax.

## Installation

SublimeLinter must be installed in order to use this plugin.

Please use [Package Control](https://packagecontrol.io) to install the linter plugin.

### Install Python 3.x

On Ubuntu/Debian for example:

    sudo apt install -y python3

### Install `cppcheck` with `misra.py` addon

Before using this plugin, you must have a version of `cppcheck` which also includes the [misra.py addon](https://github.com/danmar/cppcheck/tree/master/addons).

On Ubuntu/Debian for example:

    sudo apt install -y cppcheck

Once you have cppcheck installed, you may need to manually download the misra.py addon. First check the version of ccpcheck:

    cppcheck --version

Then download the misra.py and cppcheckdata.py addon for the printed version (`<version>`). For a Linux installation, you may want to use `wget` to save the file as follows:

```sh
    wget https://raw.githubusercontent.com/danmar/cppcheck/<version>/addons/misra.py
    wget https://raw.githubusercontent.com/danmar/cppcheck/<version>/addons/cppcheckdata.py
    sudo mkdir -p /usr/local/share/CppCheck/addons
    sudo cp -f misra.py /usr/local/share/CppCheck/addons/misra.py
    sudo cp -f cppcheckdata.py /usr/local/share/CppCheck/addons/cppcheckdata.py
```

### Generate texts from MISRA C:2012 guidelines

Due to MISRA rules, only rule check numbers are allowed in free and open source software so you need to supply your own set of texts for each rule. If you have a pdf of MISRA C:2012 guidelines, the Python 3.x script [`scripts/cppcheck-misra-parsetexts.py`](scripts/cppcheck-misra-parsetexts.py) generates the rules text file from Appendix A (Summary of guidelines).

Generate rules text:

```sh
    python3 scripts/cppcheck-misra-parsetexts.py /path/to/MISRA_C_2012.pdf
```

Now 'MISRA_C_2012_Rules.txt' should be in the '/path/to/' directory. Examine the output file for errors (there are a few parsing issues reading the PDF).

### Make sure your settings reflect installed file locations

In your project settings, set

```json
"settings": {
    "SublimeLinter.linters.cppcheck-misra.misra-addon": "/usr/local/share/CppCheck/addons/misra.py",
    "SublimeLinter.linters.cppcheck-misra.rule-texts": "/path/to/MISRA_C_2012_Rules.txt"
}
```

## Settings

- SublimeLinter settings: http://sublimelinter.readthedocs.org/en/latest/settings.html
- Linter settings: http://sublimelinter.readthedocs.org/en/latest/linter_settings.html

Additional SublimeLinter-cppcheck-misra settings:

|Setting|Description|
|:------|:----------|
|misra_addon|(Required) The misra.py addon file|
|rule_texts|(Recommended) A file of descriptions of MISRA rules|
|suppress_rules|(Optional) List of rules to ignore|
|cppcheck-opts|(Optional) Forward option to cppcheck command. Default is `"--max-configs=1"`|
|cppcheck_path|(Optional) Add search paths to find cppcheck when install location not in default PATH|

In project-specific settings, note that SublimeLinter allows [expansion variables](http://sublimelinter.readthedocs.io/en/latest/settings.html#settings-expansion). For example, the variable '${project_path}' can be used to specify a path relative to the project folder. Example settings:

```json
"settings": {
    "SublimeLinter.linters.cppcheck-misra.misra-addon": "/usr/local/share/CppCheck/addons/misra.py",
    "SublimeLinter.linters.cppcheck-misra.rule-texts": "${project_path}/misra/MISRA_C_2012_Rules.txt",
    "SublimeLinter.linters.cppcheck-misra.suppress-rules": [
        "8.14",
        "12.1"
    ],
    "SublimeLinter.linters.cppcheck-misra.cppcheck-opts": [
        "'--max-configs=1'"
    ],
    "SublimeLinter.linters.cppcheck-misra.cppcheck-path": [
        "/home/user/.local/bin"
    ]
}
```

## Contributing

If you would like to contribute enhancements or fixes, please do the following:

1. Fork the plugin repository.
1. Hack on a separate topic branch created from the latest `master`.
1. Commit and push the topic branch.
1. Make a pull request.
1. Be patient.  ;-)

Please note that modifications should follow these coding guidelines:

- Indent is 4 spaces.
- Code should pass flake8 and pep257 linters.
- Vertical whitespace helps readability, don’t be afraid to use it.
- Please use descriptive variable names, no abbreviations unless they are very well known.

Thank you for helping out!
