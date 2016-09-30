# jaffy v1
"Look ma, I made a fuzzer" edition.

*That's nice, dear.*

Jaffy fuzzes binaries that you run on the command line. You guessed it, it
fuzzes the command line options. Since it utilizes `subprocess.run()` it
requires Python 3.5 or greater. It is assumed that `/usr/local/bin/python` is
pointed to `python3.5` in some way. You can change the first line of the `jaffy`
python file to suite your needs if you want. Jaffy uses the `untangle` library
to parse XML files. You're going to need to find that, it's on `pip` and `git`.
Jaffy runs everything with `shell=True` so like don't run the fuzzer over the
internet or something. All file output is stored in a folder with the date and
time of the scan as the name, like `20160930162013`. Inside you will find the
outputs of the fuzzed binary as defined in your XML file. Each file name is a
UUID that looks something like `f718c144-f008-4fc7-96a5-164f42863868`.

Below is an example of the `chariter.xml` XML file with comments so you can see
what is going on. You should customize it to your own specifications.
```XML
<?xml version="1.0"?>
<fuzzer>
  <!-- Path to the binary -->
	<bin path="/bin/ls" />
  <!-- prestatic options are run before the fuzzed option -->
	<opt type="prestatic" value="-v" />
  <!-- poststatic options are run after the fuzzed options -->
	<opt type="poststatic" value="/home" />
  <!-- Fuzz option. Explained in readme -->
	<fuzz type="chariter" prefix="-" char="a" length="9999" />
  <!-- The shell command to run before each fuzz -->
  <prefuzz cmd="" />
  <!-- The shell command to run after each fuzz -->
	<postfuzz cmd="" />
  <!-- Decide to exit jaffy if the return code is not 0 -->
  <!-- ! = Don't exit at all. -->
  <!-- !0 = Exit only when not 0 -->
  <!-- You can also specify a shell command to run when jaffy exits -->
	<exit level="!0" cmd="" />
  <!-- Display output to the screen based on return code -->
  <!-- * = Display everything no matter what. -->
  <!-- ! = Don't display anything at all. -->
  <!-- !0 = Display only when not 0 -->
	<display level="!0" />
  <!-- Write output to file based on return code -->
  <!-- * = Write everything no matter what. -->
  <!-- ! = Don't write anything at all. -->
  <!-- !0 = Write only when not 0 -->
	<write level="!0" />
</fuzzer>
```

### Chariter Fuzz
The chariter fuzzer is the only fuzzer available in jaffy right now. It simply
creates a command line option that grows in length by one character each
iteration of the fuzz. If the `length` is `6` and the `char` is `a` then the
first fuzz will be `a`, then the second will be `aa`, and so one until you reach
`aaaaaa`. You can add a `prefix` to a each iteration as well. If your `prefix`
is `-` then your first fuzz will look like `-a`, then `-aa`, and so on.
