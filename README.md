# Incompatibility between npm 8.5.5 and 8.11.0

This project shows an error between npm 8.5.5 and 8.11.0. It appears to be a package-lock.json that was incorrectly generated with a previous version of npm, that 8.11 now fixes.

To reproduce the issue:
1) Run `nvm install v16.15.0` (or `npm install -g npm@v8.5.5`). This will install npm 8.5.5
2) Run `npm ci`. Note that it completes successfully.
3) Run `npm install`. Note that it completes successfully, and there are no `package-lock.json` changes.
4) Run `nvm install v16.15.1` (or `npm install -g npm@v8.11.0`). This will install npm 8.11.0
5) Run `npm ci`. It will fail with an error.
6) Run `npm install`. Note that it changes the package-lock.json

The error caused by step 5 running `npm ci`:
```
npm ERR! code EUSAGE
npm ERR! 
npm ERR! `npm ci` can only install packages when your package.json and package-lock.json or npm-shrinkwrap.json are in sync. Please update your lock file with `npm install` before continuing.
npm ERR! 
npm ERR! Invalid: lock file's acorn@7.4.1 does not satisfy acorn@8.7.1
npm ERR! Missing: typescript@4.7.3 from lock file
npm ERR! 
npm ERR! Clean install a project
npm ERR! 
npm ERR! Usage:
npm ERR! npm ci
npm ERR! 
npm ERR! Options:
npm ERR! [--no-audit] [--foreground-scripts] [--ignore-scripts]
npm ERR! [--script-shell <script-shell>]
npm ERR! 
npm ERR! aliases: clean-install, ic, install-clean, isntall-clean
npm ERR! 
npm ERR! Run "npm help ci" for more info

npm ERR! A complete log of this run can be found in:
npm ERR!     /home/bender/.npm/_logs/2022-06-09T16_41_51_167Z-debug-0.log
```

The diff caused by running `npm install` in step 6... might be some odd line ending issues as well?:
```
diff --git a/package-lock.json b/package-lock.json
index 560a221..2384246 100644
--- a/package-lock.json
+++ b/package-lock.json
@@ -2884,9 +2884,9 @@
       }
     },
     "node_modules/acorn": {
-      "version": "7.4.1",
-      "license": "MIT",
-      "peer": true,
+      "version": "8.7.1",
+      "resolved": "https://registry.npmjs.org/acorn/-/acorn-8.7.1.tgz",
+      "integrity": "sha512-Xx54uLJQZ19lKygFXOWsscKUbsBZW0CPykPhVQdhIeIwrbPmJzqeASDInc8nKBnp/JT6igTs82qPXz069H8I/A==",
       "bin": {
         "acorn": "bin/acorn"
       },
@@ -9514,16 +9514,6 @@
         }
       }
     },
-    "node_modules/terser/node_modules/acorn": {
-      "version": "8.7.1",
-      "license": "MIT",
-      "bin": {
-        "acorn": "bin/acorn"
-      },
-      "engines": {
-        "node": ">=0.4.0"
-      }
-    },
     "node_modules/terser/node_modules/source-map": {
       "version": "0.8.0-beta.0",
       "license": "BSD-3-Clause",
@@ -9660,6 +9650,19 @@
         "is-typedarray": "^1.0.0"
       }
     },
+    "node_modules/typescript": {
+      "version": "4.7.3",
+      "resolved": "https://registry.npmjs.org/typescript/-/typescript-4.7.3.tgz",
+      "integrity": "sha512-WOkT3XYvrpXx4vMMqlD+8R8R37fZkjyLGlxavMc4iB8lrl8L0DeTcHbYgw/v0N/z9wAFsgBhcsF0ruoySS22mA==",
+      "peer": true,
+      "bin": {
+        "tsc": "bin/tsc",
+        "tsserver": "bin/tsserver"
+      },
+      "engines": {
+        "node": ">=4.2.0"
+      }
+    },
     "node_modules/unherit": {
       "version": "1.1.3",
       "license": "MIT",
@@ -10206,16 +10209,6 @@
         "node": ">= 10.13.0"
       }
     },
-    "node_modules/webpack-bundle-analyzer/node_modules/acorn": {
-      "version": "8.7.1",
-      "license": "MIT",
-      "bin": {
-        "acorn": "bin/acorn"
-      },
-      "engines": {
-        "node": ">=0.4.0"
-      }
-    },
     "node_modules/webpack-bundle-analyzer/node_modules/ansi-styles": {
       "version": "4.3.0",
       "license": "MIT",
@@ -10470,16 +10463,6 @@
         "node": ">=10.13.0"
       }
     },
-    "node_modules/webpack/node_modules/acorn": {
-      "version": "8.7.1",
-      "license": "MIT",
-      "bin": {
-        "acorn": "bin/acorn"
-      },
-      "engines": {
-        "node": ">=0.4.0"
-      }
-    },
     "node_modules/webpackbar": {
       "version": "5.0.2",
       "license": "MIT",
@@ -12604,8 +12587,9 @@
       }
     },
     "acorn": {
-      "version": "7.4.1",
-      "peer": true
+      "version": "8.7.1",
+      "resolved": "https://registry.npmjs.org/acorn/-/acorn-8.7.1.tgz",
+      "integrity": "sha512-Xx54uLJQZ19lKygFXOWsscKUbsBZW0CPykPhVQdhIeIwrbPmJzqeASDInc8nKBnp/JT6igTs82qPXz069H8I/A=="
     },
     "acorn-import-assertions": {
       "version": "1.8.0",
@@ -16515,9 +16499,6 @@
         "source-map-support": "~0.5.20"
       },
       "dependencies": {
-        "acorn": {
-          "version": "8.7.1"
-        },
         "source-map": {
           "version": "0.8.0-beta.0",
           "requires": {
@@ -16613,6 +16594,12 @@
         "is-typedarray": "^1.0.0"
       }
     },
+    "typescript": {
+      "version": "4.7.3",
+      "resolved": "https://registry.npmjs.org/typescript/-/typescript-4.7.3.tgz",
+      "integrity": "sha512-WOkT3XYvrpXx4vMMqlD+8R8R37fZkjyLGlxavMc4iB8lrl8L0DeTcHbYgw/v0N/z9wAFsgBhcsF0ruoySS22mA==",
+      "peer": true
+    },
     "unherit": {
       "version": "1.1.3",
       "requires": {
@@ -16903,11 +16890,6 @@
         "terser-webpack-plugin": "^5.1.3",
         "watchpack": "^2.3.1",
         "webpack-sources": "^3.2.3"
-      },
-      "dependencies": {
-        "acorn": {
-          "version": "8.7.1"
-        }
       }
     },
     "webpack-bundle-analyzer": {
@@ -16924,9 +16906,6 @@
         "ws": "^7.3.1"
       },
       "dependencies": {
-        "acorn": {
-          "version": "8.7.1"
-        },
         "ansi-styles": {
           "version": "4.3.0",
           "requires": {
```
