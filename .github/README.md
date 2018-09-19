# BIASlab METADATA repository

This is the private METADATA repository for the Julia package manager for members of BIASlab. It is kept up-to-date with the upstream repository as much as possible, but also contains our private packages which (at least currently) cannot be released to the general public.

To use this repository issue `Pkg.init("ssh://git@github.com/biaslab/METADATA.jl.git", "master")` from inside Julia. *NOTE:* This command will only succeed if the package repository has not yet been initialized through any of the `Pkg` commands (e.g. `resolve`, `update`, etc.). If a package repository has already been initialized, either remove it (e.g. `rm ~/.julia/v0.x`, substituting the proper Julia version) or back it up somewhere safe in case you have anything in it that you cannot live without.

When adding a new (private) repository, make sure to set its URL in the `url` file to one of the form `ssh://git@github.com/organization/repository.jl.git`, to decrease issues with the private nature of this METADATA repository when cloning the packages within! Also, add the repository name to the list of (private) BIASlab packages that are registered in the master branch. **Please keep this list up to date**.

- ArgmaxBandit.jl
- AudiogramPriors.jl
- AudioIO.jl (our private fork)
- BayesianPTA.jl
- FLEXpecs.jl
- ForneyLab.jl
- GaussianProcessLab.jl
- HearingAid.jl
- ParameterizedTypes.jl
- PHLC.jl
- TinyBShowcase.jl

## Pulling in changes from the upstream repository

Packages available in the upstream repository will typically quickly become out-of-date with the private repository. To remedy this, they have to be synchronized. The steps to do so are documented below.

### Initial Setup

To be able to synchronize the two repositories a copy of the private repository is needed with the upstream repository added as an additional remote. From scratch this can be achieved using
```shell
$ git clone git@github.com:biaslab/METADATA.jl.git
$ cd METADATA.jl
$ git remote add upstream git@github.com:JuliaLang/METADATA.jl.git
$ git branch metadata-v2 origin/metadata-v2
```

### Performing Synchronization

```shell
$ git fetch --all
$ git checkout metadata-v2
$ git pull upstream metadata-v2
$ git checkout master
# Make sure your current master branch is up-to-date!
$ git pull --rebase origin master
$ git merge metadata-v2
$ git push origin
```

Or execute the `synchronize_with_upstream` script within a properly configured repository.

## Original README

This is the official METADATA repo for the Julia package manager. See [manual section](https://docs.julialang.org/en/latest/stdlib/Pkg/) on packages for how to use the package manager to install and develop packages.

Please note our current policies for accepting entries into METADATA.jl:

1. Registered packages must have an [Open Source Initiative approved license](http://opensource.org/licenses), clearly marked via a `LICENSE.md`, `LICENSE`, `COPYING` or similarly named file in the package repository. Packages that wrap proprietary libraries are acceptable if the licenses of those libraries permit open source distribution of the Julia wrapper code.
2. New packages submitted for registration must have at least one tagged version.
3. The lowest package version that will be accepted is v0.0.1. v0.0.0 is no longer permitted.
4. All new tagged versions of packages must have a `REQUIRE` file, which must at a minimum contain a single line like
   ```
   julia 0.6
   ```
   specifying a minimum version of Julia the package is expected to run on. Running `Pkg.tag` copies the contents of a package's `REQUIRE` file into `METADATA.jl/PkgName/versions/1.2.3/requires`.

   A common mistake is to have an entry of the form
   ```
   julia 0.6-
   ```
   with the intention of specifying "version 0.4 and up." On the contrary, this line means "at least a 0.4 pre-release julia."
5. New package version tags must have a minimum Julia version of `0.5` or newer. `0.5-` (0.5 pre-releases) is no longer allowed.
   Exceptions may be granted for `julia 0.4` if package authors are willing to vouch that they still test that their packages work on 0.4.
6. If your package works with Julia 0.6 but not 0.5, then specify `julia 0.6` in your `REQUIRE` file. If the package has had any previous   tags which supported `julia 0.5`, then be sure to change the minor or major version number of the package via `Pkg.tag("PkgName", :minor)` for the first tag that no longer supports `julia 0.5`. This makes it possible to create a separate branch for any future bugfix releases that may be needed for the package on Julia 0.5.
7. We strongly encourage everyone to update METADATA.jl through pull requests, which can be generated for you automatically when you tag a package using Github's UI, provided you have [attobot](https://github.com/integration/attobot) enabled on your repository. Alternatively, you can use the [PkgDev](https://github.com/JuliaLang/PkgDev.jl) package, and its `PkgDev.publish()` function to create PRs. GitHub's pull requests allow us to run basic checks on the metadata entries. METADATA.jl should not be edited directly unless absolutely necessary in an emergency.
8. Do not modify the `sha1` files of existing tags after they have been published by merging to the `JuliaLang/metadata-v2` branch. Bounds can be modified in the `requires` files after the fact, but the code content should remain unchanged for reproducibility of past results.

These policies have been the result of many months of discussion to improve the quality of registered packages and the overall user experience with Julia packages.