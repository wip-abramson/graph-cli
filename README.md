# The Graph CLI (graph-cli)

[![npm (scoped)](https://img.shields.io/npm/v/@graphprotocol/graph-cli.svg)](https://www.npmjs.com/package/@graphprotocol/graph-cli)
[![Build Status](https://travis-ci.org/graphprotocol/graph-cli.svg?branch=master)](https://travis-ci.org/graphprotocol/graph-cli)

The Graph command line interface.

As of today, the command line interface consists of two commands:

- `graph codegen` — generates TypeScript code for smart contract ABIs used in subgraphs.
- `graph build` — compiles subgraphs to WebAssembly and deploys them to IPFS.

## How It Works

`graph` takes a `subgraph.yaml` subgraph manifest with

- references to a GraphQL schema,
- smart contract ABIs, and
- mappings written in TypeScript/AssemblyScript,

compiles the mappings to WebAssembly and deploys a ready-to-use
version of the subgraph to IPFS or a local directory for debugging.

## Usage

Subgraphs for The Graph are set up just like any regular TypeScript
project. It is recommended to install `graph-cli` as a local dependency
via `package.json` and use `npm` scripts for code generation and
building.

If you are just getting started with creating a subgraph, head to [The Getting Started Guide](https://github.com/graphprotocol/graph-node/blob/master/docs/getting-started.md). Eventually that guide will lead you back here.

For clarity, an example of the setup below can be found in the [Ens subgraph repository](https://github.com/graphprotocol/ens-subgraph).

### Steps 

1.  Create a project for the subgraph with a `package.json` etc.
2.  Add a `subgraph.yaml` subgraph manifest with a GraphQL schema etc.
3.  Add `@graphprotocol/graph-cli` and `@graphprotocol/graph-ts` dependencies, with one of

    ```bash
    # NPM
    npm install --save-dev
      @graphprotocol/graph-cli \
      @graphprotocol/graph-ts

    # Yarn
    yarn add --dev \
      @graphprotocol/graph-cli \
      @graphprotocol/graph-ts
    ```

4.  Add the following `tsconfig.json`:
    ```json
    {
      "extends": "./node_modules/@graphprotocol/graph-ts/tsconfig.json",
      "compilerOptions": {
        "types": ["@graphprotocol/graph-ts"]
      }
    }
    ```
5.  Add the following to `package.json`:
    ```json
    {
      "scripts": {
        "codegen": "graph generate-types subgraph.yaml",
        "build": "graph build subgraph.yaml",
        "build-ipfs": "graph build --ipfs /ip4/127.0.0.1/tcp/5001 subgraph.yaml",
        "deploy":
          "graph deploy --ipfs /ip4/127.0.0.1/tcp/5001 --node http://127.0.0.1:8030 --subgraph-name your-subgraph subgraph.yaml"
      }
    }
    ```
    _Note: Replace the IP addresses and ports with any
    [Graph Node](https://github.com/graphprotocol/graph-node) you want
    to deploy the subgraph to._
6.  Generate type definitions for contract ABIs used in the subgraph
    with:
    ```bash
    yarn codegen
    ```
7.  Develop your `mapping.ts` against these generated types. If you haven't developed your mappings yet, you can head to [The Getting Started Doc](https://github.com/graphprotocol/graph-node/blob/master/docs/getting-started.md#34-write-your-mappings). 
8.  Build the subgraph with one of
    ```sh
    yarn build      # Will drop the results in dist/
    yarn build-ipfs # Will also deploy to IPFS and output an IPFS hash
    ```
9.  Deploy your subgraph to a
    [Graph Node](https://github.com/graphprotocol/graph-node). The following
    command builds and deploys the subgraph continuously as you are making
    changes to it:
    ```sh
    graph \
       --watch \
       --verbosity debug \
       --node http://127.0.0.1:8020/ \
       --ipfs /ip4/127.0.0.1/tcp/8030 \
       --subgraph-name your-subgraph \
       deploy \
       subgraph.yaml
    ```

## License

Copyright &copy; 2018 Graph Protocol, Inc. and contributors.

The Graph CLI is dual-licensed under the [MIT license](LICENSE-MIT) and the
[Apache License, Version 2.0](LICENSE-APACHE).

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
