## Basic

### Directory structure
```
.
├── bin
│   └── closure (SoyToJsSrcCompiler.jar, XtbGenerator.jar, compiler.jar)
│
└── www
    ├── css
    │
    ├── js
    │   ├── DIRECTORY_1 (example application directory)
    │   ├── DIRECTORY_2
    │   │
    │   ├── app.js (application entry point)
    │   └── ... (app-compiled.js, app-deps.js, app-messages.js)
    │
    ├── js-closure (cloned closure GIT repository)
    │   └── closure
    │       ├── bin
    │       └── goog
    │
    ├── js-externs
    │
    ├── js-soy (soyutils_usegoog.js)
    │
    └── index.html
```

### BASH variables
Relative to ```./bin``` directory
```bash
PYTHON_BIN="python"
JVM_ARCH="-d64"
CLOSURE_BUILD_DIR="../www/js-closure/closure/bin/build"
CLOSURE_UTIL_DIR="closure"

JS_APP_DIR="../www/js"
JS_APP_INPUT="../www/js/app.js"

JS_APP_COMPILED="../www/js/app-compiled.js"
JS_APP_DEPS="../www/js/app-deps.js"
JS_APP_MESSAGES="../www/js/app-messages.xtb"

JS_CLOSURE_LIB_DIR="../www/js-closure/closure/goog"
JS_CLOSURE_THIRD_PARTY_DIR="../www/js-closure/third_party/closure/goog"
JS_CLOSURE_SOY_DIR="../www/js-soy"

TEMPLATE_DIR="../www/js"

URL_APP="../../../js"
URL_SOY="../../../js-soy"

LANG="cs"
LOCALE="cs_CZ"
```

### index-devel.html
```html
<!doctype html>
<html>
<head>
	<script src="/js-closure/closure/goog/base.js"></script>
	<script src="/js/app-deps.js"></script>
	<script>
		goog.require('app.start');
	</script>
</head>
<body>
	<!-- HTML CONTENT -->
	<script>
		app.start();
	</script>
</body>
</html>
```

### index-prod.html
```html
<!doctype html>
<html>
<head>
	<script src="/js/app-compiled.js"></script>
</head>
<body>
	<!-- HTML CONTENT -->
	<script>
		app.start();
	</script>
</body>
</html>
```


### Deps
Scan and generate ```./www/js/app-deps.js``` (dependencies) based on ```goog.provide()``` and ```goog.require()```

```bash
${PYTHON_BIN} ${CLOSURE_BUILD_DIR}/depswriter.py \
	--root_with_prefix="${JS_APP_DIR} ${URL_APP}" \
	--root_with_prefix="${JS_CLOSURE_SOY_DIR} ${URL_SOY}" \
	--output_file=${JS_APP_DEPS}
```

### Templates (soy)
Find all templates ```*.soy``` and compile it to JS source.

```bash
java -jar ${CLOSURE_UTIL_DIR}/SoyToJsSrcCompiler.jar \
	--outputPathFormat {INPUT_DIRECTORY}/{INPUT_FILE_NAME}.js \
	--shouldGenerateJsdoc \
	--shouldProvideRequireSoyNamespaces \
	--shouldGenerateGoogMsgDefs \
	--bidiGlobalDir 1 \
	--srcs $(find ${TEMPLATE_DIR} -iname '*.soy' -type f -print0 | xargs -0 echo)
```

### Messages
Generate XTB translation file ```./www/js/app-messages.xtb```

```bash
${PYTHON_BIN} ${CLOSURE_BUILD_DIR}/closurebuilder.py \
	--root=${JS_APP_DIR} \
	--root=${JS_CLOSURE_LIB_DIR} \
	--root=${JS_CLOSURE_THIRD_PARTY_DIR} \
	--root=${JS_CLOSURE_SOY_DIR} \
	--input=${JS_APP_INPUT} \
	--output_mode=compiled \
	--compiler_jar=${CLOSURE_UTIL_DIR}/XtbGenerator.jar \
	--jvm_flags="${JVM_ARCH}" \
	--compiler_flags="--translations_file=${JS_APP_MESSAGES}" \
	--compiler_flags="--xtb_output_file=${JS_APP_MESSAGES}" \
	--compiler_flags="--lang=${LANG}"
```

### Compile
Compile application into ```./www/js/app-compiled/js```

```bash
${PYTHON_BIN} ${CLOSURE_BUILD_DIR}/closurebuilder.py \
	--root=${JS_APP_DIR} \
	--root=${JS_CLOSURE_LIB_DIR} \
	--root=${JS_CLOSURE_THIRD_PARTY_DIR} \
	--root=${JS_CLOSURE_SOY_DIR} \
	--input=${JS_APP_INPUT} \
	--output_mode=compiled \
	--compiler_jar=${CLOSURE_UTIL_DIR}/compiler.jar \
	--jvm_flags="${JVM_ARCH}" \
	--compiler_flags="--compilation_level=ADVANCED_OPTIMIZATIONS" \
	--compiler_flags="--warning_level=verbose" \
	--compiler_flags="--define=goog.DEBUG=false" \
	--compiler_flags="--define=goog.LOCALE='${LOCALE}'" \
	--output_file=${JS_APP_COMPILED}
```

### Externs

If you are using another JS lib in your application, you need to provide ```externs``` option to compiler. The file must contain annotation definition for each function, object, ...
```bash
${PYTHON_BIN} ${CLOSURE_BUILD_DIR}/closurebuilder.py \
	...
	--output_mode=compiled \
	--compiler_jar=${CLOSURE_UTIL_DIR}/compiler.jar \
	--compiler_flags="--externs=${EXTERNS_DIR}/externs.js"
	...
```

Mostly used ```externs``` is already on [compiler externs](https://github.com/google/closure-compiler/tree/master/contrib/externs)

### Source map

Generating source map (for debugging compiled application)
```bash
${PYTHON_BIN} ${CLOSURE_BUILD_DIR}/closurebuilder.py \
	...
	--output_mode=compiled \
	--compiler_jar=${CLOSURE_UTIL_DIR}/compiler.jar \
	--compiler_flags="--create_source_map=../www/js/source.map" \
	...
```

Add comment to end of ```./www/js/app-compiled.js```
```javascript

//@ sourceMappingURL=/js/source.map
```

And fix path in source map (replace ```../www/``` to ```/```)
```bash
%s/..\/www\//\//g
```

## Basic example
Complete skeleton/example:

1. Download [closure-example-bash.tar.gz](/files/closure-example-bash.tar.gz)

2. Add Closure Library into ```www/js-closure```<br />(e.g.: run ```git clone https://github.com/google/closure-library.git www/js-closure```)

Development:
```bash
# compile SOY templates - when you change SOY template
./bin/build.sh soy

# generate JS dependencies - when you add/change goog.require or goog.provide
./bin/build.sh deps

# generate messages - when you add/change goog.getMsg
./bin/build.sh messages
```

Production:
```bash
# compile application - js/app-compiled.js
./bin/build.sh build
```
