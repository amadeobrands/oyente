# Oyente

**Note: The tool is currently under development, please report any bugs you may find.**

## Quick Start

A container with the dependencies set up and the blockchain snapshot installed can be found [here](https://hub.docker.com/r/hrishioa/oyente/).

Note - [This](https://github.com/melonproject/oyente/tree/290f1ae1bbb295b8e61cbf0eed93dbde6f287e69) is the version of Oyente that is shipped with the docker container. If you want to run the most recent version, follow the full installation instructions [below](#full-install). 

To open the container, install docker and run:

```
docker pull hrishioa/oyente && docker run -i -t hrishioa/oyente
```

To evaluate the greeter contract inside the container, run:

```
cd /home/oyente/oyente && source ../dependencies/venv/bin/activate && python oyente.py -s greeter.sol
```

and you are done!

## Full install

### Install the following dependencies
#### solc v0.4.10
```
$ sudo add-apt-repository ppa:ethereum/ethereum
$ sudo apt-get update
$ sudo apt-get install solc
```

#### evm from [go-ethereum](https://github.com/ethereum/go-ethereum) version 1.6.1. 

1. https://geth.ethereum.org/downloads/ or
2. By from PPA if your using Ubuntu
```
$ sudo apt-get install software-properties-common
$ sudo add-apt-repository -y ppa:ethereum/ethereum
$ sudo apt-get update
$ sudo apt-get install ethereum
```

#### [z3](https://github.com/Z3Prover/z3/releases) Theorem Prover version 4.4.1.

Download the [source code of version z3-4.4.1](https://github.com/Z3Prover/z3/releases/tag/z3-4.4.1)

Install z3 using Python bindings

```
$ python scripts/mk_make.py --python
$ cd build
$ make
$ sudo make install
```

#### [Requests](https://github.com/kennethreitz/requests/) library

```
pip install requests
```

#### [web3](https://github.com/pipermerriam/web3.py) library

```
pip install web3
```

### Evaluating Ethereum Contracts

```
#evaluate a local solidity contract
python oyente.py -s <contract filename>

#evaluate a local evm contract
python oyente.py -s <contract filename> -b

#evaluate a remote contract
python oyente.py -ru https://gist.githubusercontent.com/loiluu/d0eb34d473e421df12b38c12a7423a61/raw/2415b3fb782f5d286777e0bcebc57812ce3786da/puzzle.sol

```

And that's it! Run ```python oyente.py --help``` for a list of options.

## Paper

The accompanying paper explaining the bugs detected by the tool can be found [here](http://www.comp.nus.edu.sg/~loiluu/papers/oyente.pdf).

## Miscellaneous Utilities

A collection of the utilities that were developed for the paper are in `misc_utils`. Use them at your own risk - they have mostly been disposable.

1. `generate-graphs.py` - Contains a number of functions to get statistics from contracts.
2. `get_source.py` - The *get_contract_code* function can be used to retrieve contract source from [EtherScan](https://etherscan.io)
3. `transaction_scrape.py` - Contains functions to retrieve up-to-date transaction information for a particular contract.

## Benchmarks

Note: This is an improved version of the tool used for the paper. Benchmarks are not for direct comparison.

To run the benchmarks, it is best to use the docker container as it includes the blockchain snapshot necessary.
In the container, run `batch_run.py` after activating the virtualenv. Results are in `results.json` once the benchmark completes.

The benchmarks take a long time and a *lot* of RAM in any but the largest of clusters, beware.

Some analytics regarding the number of contracts tested, number of contracts analysed etc. is collected when running this benchmark.

#### Known Issues
If you encounter the `unhashable instance` error, please add the following to your `class AstRef(Z3PPObject):` in `/usr/lib/python2.7/dist-packages/z3.py`:
```
def __hash__(self):
        return self.hash()
```
The latest version of Z3 does support this, but some previous version does not.
