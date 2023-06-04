# shell-help-utils
For ease of making tons of help scripts for reference

## What's it do?
This lets you make informative help files with great ease,

1. You just put your snippet in an executable file
2. Choose the filename for convenient *shell tab-completion*
3. Add one line (a *'shebang'*) to the start of the file (see Example)
4. Now you have
    1. Tab completable references
    2. Prompted to copy it to your clipboard
    3. Syntax highlighting (that's disabled if the output is not a term)
    4. A choice of `-l language` or letting `display_file_shell` examine your own shebang (on the 2nd line of the file)

## Example(s)

### Say I have a python argparse example I want as a reference. I put it in my bin/ directory in a file `python-help-argparse`

1. This is what you put in your helpfile:
```python
#!/usr/bin/env -S display_file_shell python
import argparse
parser = argparse.ArgumentParser()
parser.add_argument("x", type=int, help="the base")
parser.add_argument("y", type=int, help="the exponent")
parser.add_argument("-w", "--wide", help="make something wide",
                    action="store_true")
parser.add_argument("-v", "--verbosity", action="count",
                    default=0, help="increase output verbosity")
#parser.add_argument('text', nargs='?', help='Non-option argument')
args = parser.parse_args()
```
2. If you use a *shebang* in your code example, it can be detected so a -l {syntax} specifier is not needed [for syntax highlighting]:
```bash
#!/usr/bin/env display_file_shell
#!/bin/bash
ourdir="$(dirname "$(readlink -f "$0")")"
ourname="${ourdir##*/}"

if [[ "$#" -lt 1 ]]; then
	cat <<-"EOT"
		Usage: $ourname video.mp4 [framecount]
		* To just show information, don't specify a framecount
		EOT
elif [[ "$#" -eq 1 ]]; then
	"$bin_vid" "$vidfn" # bin_vid not included in this example
fi
```

## Requirements
* `highlight`
* ...or `syntax-highlight`. But you need `display_file_shell -s` to tell it to use that (we don't detect if highlight is missing currently)

## Usage
1. Clone repo or just get `display_file_shell`
2. Include it in your path somewhere (I usually just symlink to things, like:
    * `cd ~/bin`
    * `ln -s somewhere/repo/display_file_shell`
3. Put your snippets in a file in your path
4. Add a shebang: `#!/usr/bin/env -S display_file_shell {syntax/language}

## Known limitations
* For 2nd line shebang detection: We handle this manually, and only currently look for bin/bash, bin/perl, and *python*
