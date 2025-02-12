- [Medium doc Patterns for accessing code from Dynamic Feature Modules](https://medium.com/androiddevelopers/patterns-for-accessing-code-from-dynamic-feature-modules-7e5dca6f9123)


Dynamic code loading sample
============

This sample demonstrates how to load and use classes from dynamic feature modules.

Introduction
------------
Dynamic feature modules (DFMs) are modularized parts of your Android app. You will encounter them when you switch 
to using Android App Bundles and want to enable on-demand or conditional feature delivery. 

You can read more about app bundles at http://g.co/androidappbundle

Contrary to regular library modules, DFMs depend on the base module (`com.android.application`) of your app. 
Because of this, classes defined in DFMs are not visible from the app module at compile time and need to be accessed 
through reflection.

This sample demonstrates three different approaches to safely access code from the feature module 
using at most 1 reflection call. 

Common code is included in the `main` sourceSets, which contains the basic UI and ViewModel for this sample.
This also contains the `StorageFeature` interface (defined in the `app` module), 
which will be implemented in the `storage` dynamic feature module.  

The other sourceSets contain code that obtains an instance of the concrete implementation from the DFM 
using three approaches, split into the following product flavors:

 `reflect/` -> uses a straight reflection call
 `serviceLoader/` -> uses the [ServiceLoader](https://developer.android.com/reference/java/util/ServiceLoader) mechanism
 `dagger/` -> uses Dagger 2, to enable easy instantiation of complex object graphs from the DFM
 
Please note that all approaches use 1 reflect call to instantiate the `StorageFeature.Provider`, 
although in the `serviceLoader` and `dagger` approaches they're somewhat hidden from the user.
 
Additionally, when compiling the `serviceLoader` flavor using recent versions of R8, 
the optimizer does away with dynamic class lookup and reflection, and the resulting DEX code uses direct class 
instantiation.

Pre-requisites
--------------

* Android Studio 3.4
* Access to Play Console in order to test the on-demand features

Getting Started
---------------

Use Android Studio to open the project and check out the various Build Variants available.

Alternatively, build from the commandline using `./gradlew reflectDebug` 
or `./gradlew serviceLoaderDebug` or `./gradlew daggerDebug`

Support
-------

If you've found an error in this sample, please file an issue:
https://github.com/googlesamples/android-dynamic-code-loading/issues

Patches are encouraged, and may be submitted by forking this project and
submitting a pull request through GitHub.

License
-------

```
Copyright 2019 Google LLC.

Licensed to the Apache Software Foundation (ASF) under one or more contributor
license agreements.  See the NOTICE file distributed with this work for
additional information regarding copyright ownership.  The ASF licenses this
file to you under the Apache License, Version 2.0 (the "License"); you may not
use this file except in compliance with the License.  You may obtain a copy of
the License at

  http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.  See the
License for the specific language governing permissions and limitations under
the License.
```
