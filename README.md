# Centreon Connectors #

Centreon Connectors are extremely fast open-source monitoring check
execution daemons designed to work with
[Centreon Engine](https://github.com/centreon/centreon-engine).
It is a low-level component of the [Centreon software suite](https://www.centreon.com).

Centreon Connectors are released under the Apache Software License version 2
and is endorsed by the [Centreon company](https://www.centreon.com).

There are currently two open-source connectors :

* **Centreon Connector Perl** : persistent Perl interpreter that
  executes Perl plugins very fast
* **Centreon Connector SSH** : maintain SSH connexions opened to reduce
  overhead of plugin execution over SSH

## Documentation ##

The full Centreon Connector Perl documentation is available online
[here](http://documentation.centreon.com/docs/centreon-perl-connector/en/).
The full Centreon Connector SSH documentation is also available online
[here](http://documentation.centreon.com/docs/centreon-ssh-connector/en/).
The two documentation sets are generated from ReST files located in the
*./perl/doc/* and *./ssh/doc/* directories (respectively) of Centreon
Connectors sources.

Documentation extensively covers all aspects of Centreon Connectors such
as installation, compilation, configuration, use and more. It is the
reference guide of the software. This *README* is only provided as a
quick introduction.

## Installing from binaries ##

**Warning**: Centreon Connectors are low-level components of the
Centreon software suite. If this is your first installation you would
probably want to [install it entirely](https://documentation.centreon.com/docs/centreon/en/2.6.x/installation/index.html).

Centreon ([the company behind the Centreon software suite](http://www.centreon.com))
provides binary packages for RedHat / CentOS. They are available either
as part of the [Centreon Entreprise Server distribution](https://www.centreon.com/en/products/centreon-enterprise-server/)
or as individual packages on [our RPM repository](https://documentation.centreon.com/docs/centreon/en/2.6.x/installation/from_packages.html).

Once the repository installed a simple command will be needed to install
a connector.

    $# yum install centreon-connector-perl
    $# yum install centreon-connector-ssh

## Fetching sources ##

The reference repository is hosted at [GitHub](https://github.com/centreon/centreon-connectors).
Beware that the repository hosts in-developement sources and that it
might not work at all.

Stable releases are available as gziped tarballs on [Centreon's download site](https://download.centreon.com).

## Compilation (quickstart) ##

**Warning**: Centreon Connectors are low-level components of the
Centreon software suite. If this is your first installation you would
probably want to [install it entirely](https://documentation.centreon.com/docs/centreon/en/2.6.x/installation/index.html).

This paragraph is only a quickstart guide for the compilation of
Centreon Connectors. For a more in-depth guide with build options you should
refer to the online documentation of
[Centreon Connector Perl](http://documentation.centreon.com/docs/centreon-perl-connector/en/)
or [Centreon Connector SSH](http://documentation.centreon.com/docs/centreon-ssh-connector/en/).

Centreon Connectors need Centreon Clib to be build. You should
[install it first](https://github.com/centreon/centreon-clib).

### CentOS / Debian / Raspbian

Compilation of this distribution is pretty straightforward.

You'll need to download the project and launch the *cmake.sh* script
to prepare the compilation environment:

```shell
git clone https://github.com/centreon/centreon-connectors
cd centreon-connectors
./cmake.sh
```

Now launch the compilation using the *make* command and then install the
software by running *make install* as priviledged user:

```shell
cd build
make
make install
```

### Other distributions

If you are on another distribution, then follow the steps below.

Check if you have these packages installed (Note that packages names
come from CentOS distributions, so if some packages names don't match
on your distribution try to find their equivalent names): git, make,
cmake, gcc-c++.

For the projet compilation you need to have conan installed. Try to use
the package manager given by your OS to install conan. ('apt' for
Debian, 'rpm' for Red Hat, 'pacman' for Arch Linux, ...). It is prefered
to install gcc before conan.

Example:

```shell
apt install conan
```

If it does not work, conan can be installed with pip3:

```shell
pip3 install conan
```

> All the dependencies pulled by conan are located in conanfile.txt. If
> you want to use a dependency from your package manager instead of conan,
> you need to remove it from conanfile.txt.

You can now prepare the compilation environment:

```shell
git clone https://github.com/centreon/centreon-connectors
mkdir -p centreon-connectors/build
cd centreon-connectors/build
conan install --build missing ..
cmake -DCMAKE_BUILD_TYPE=Release -DWITH_PREFIX_BINARY=/usr/lib64/centreon-connector -DWITH_TESTING=On ..
```

This will look for required dependencies and print a summary of the
compilation parameters if everything went fine.

Now launch the compilation using the *make* command and then install the
software by running *make install* as priviledged user:

```shell
make
make install
```

You're done!

## Bug reports / Feature requests

The best way to report a bug or to request a feature is to open an issue
in GitHub's [issue tracker](https://github.com/centreon/centreon-connectors/issues/).

Please note that Centreon Connectors follows the
[same workflow as Centreon](https://github.com/centreon/centreon/blob/master/project/issues.md)
to process issues.

For a quick resolution of a bug your message should contain :

* the problem description
* precise steps on how to reproduce the issue (if you're using Centreon
  web UI tell us where you click)
* the expected behavior
* the Centreon product**s** version**s**
* the operating system you're using (name and version)
* if possible configuration, log and debug files

## Contributing ##

Contributions are much welcome ! If possible provide them as
pull-requests on GitHub. If not, patches will do but describe against
which vesion/commit they apply.

For any question or remark feel free to send a mail to the project
maintainer : Matthieu Kermagoret (mkermagoret@centreon.com).
