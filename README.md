# Yarn Typescript Monorepo

Welcome to the Yarn Typescript Monorepo Template! This guide will walk you through the steps to set up the repository after cloning it, and highlight the features supported by this template.

## Features

This monorepo template comes with a variety of features to streamline development across multiple packages:

### Yarn Workspaces

- Efficiently manage dependencies across multiple packages.
- Share dependencies to reduce disk space usage and improve installation times.

### TypeScript Based

- Write your packages using TypeScript for type safety and better developer experience.
- Centralized TypeScript configuration in the root, with package-specific overrides if needed.

### ESM Import/Export

- Use ESM syntax for importing and exporting modules.

### Top-Level Await

- Use top-level await in Node.js scripts.

### Linting and Formatting

- Pre-configured ESLint and Prettier setups to ensure a consistent code style across all packages.
- Shared linting rules with the ability to extend or override in individual packages.

## Getting Started

### Requirements

Before you begin, ensure you have the following installed on your machine:

- Node.js `>=20.12.2`: It is recommended to use [fnm](https://github.com/Schniz/fnm) to manage Node.js versions.
- Yarn `>=1.22.22`: You can use `$ corepack enable` to enable Node.js corepack if you don't have `yarn` installed.

### Cloning the Repository

First, clone the repository to your local machine:

```sh
git clone git@github.com:OoadadaoO/yarn-monorepo.git
mv yarn-monorepo <your-repo-name>
cd <your-repo-name>
rm -rf .git
# If you want to start a new git repository
git init
git branch -m main
```

### Setting Up the Project

This template uses Yarn workspaces to manage dependencies across multiple packages. Follow these steps to set up the project:

### Initialization

1.  **Update Name**: Update the package name in the following files:

    [root package.json](package.json)

    ```json
    {
      "scripts": {
        "pkg-repo": "yarn workspace <project_name>/pkg-repo",
        "app-repo": "yarn workspace <project_name>/app-repo"
      }
    }
    ```

    [app-repo package.json](apps/app-repo/package.json)

    ```json
    {
      "name": "<project_name>/app-repo"
    }
    ```

    [pkg-repo package.json](packages/pkg-repo/package.json)

    ```json
    {
      "name": "<project_name>/pkg-repo"
    }
    ```

    [app-repo index.ts](apps/app-repo/src/index.ts)

    ```ts
    import * as pkg from "<project_name>/pkg-repo";
    ```

1.  **Install Dependencies**: Navigate to the root of the repository and run:

    ```sh
    yarn install
    ```

    This command will install all dependencies for the root package as well as for each workspace package defined in the `package.json`.

### Testing

1. **Monorepo Import**: Build and run the package and its dependencies of `app-demo` by running:

   ```sh
   yarn app-repo build && yarn app-repo start
   ```

2. **Run Linter**: Run ESLint for files in `apps` and `packages`:

   ```sh
   yarn lint
   ```

   See more details in the [.eslintrc.cjs](.eslintrc.cjs) file.

3. **Run Formatter**: Run Prettier for all src directory of packages in `apps` and `packages`:

   ```sh
   yarn format
   ```

   See more details in the [.prettierrc.cjs](.prettierrc.cjs) file.

### Adding New Apps or Packages

The following steps will demonstrate in the context of a new package. The same steps can be applied to a new app just by using the `apps` directory instead of the `packages` directory.

1. Create a new package:

   ```sh
   cp -r packages/pkg-repo packages/<new-package-name>
   ```

2. Setup package name:

   In `./packages/<new-package-name>/package.json`

   ```json
   {
     "name": "<project_name>/<new-package-name>"
   }
   ```

   In `./package.json`

   ```json
   {
     "scripts": {
       "<new-package-name>": "yarn workspace <project_name>/<new-package-name>"
     }
   }
   ```

3. Setup monorepo dependencies:

   In `./packages/<new-package-name>/tsconfig.json`

   ```json
   {
     "references": [{ "path": "../../packages/<your-package-name>" }]
   }
   ```

Now, you can run yarn script as usual but replace `yarn` to `yarn <new-package-name>`. like:

```sh
yarn <new-package-name> add <dependency>
yarn <new-package-name> dev
yarn <new-package-name> build
yarn <new-package-name> start
```

Customize the your yarn script in `./packages/<new-package-name>/package.json` as needed.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.

---

Thank you for using our monorepo template! If you have any questions or run into any issues, please feel free to open an issue on GitHub.
