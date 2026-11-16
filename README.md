# mecab-ipadic for Lindera

This repository holds the files of the mecab-ipadic release 2.7.0-20070801,
with the lexicon and most of the definition files converted from EUC-JP to
UTF-8. mecab-ipadic is the IPA dictionary for Japanese morphological analysis
with [MeCab](https://taku910.github.io/mecab/), and
[Lindera](https://github.com/lindera/lindera) builds its Japanese dictionary,
`lindera-ipadic`, from this repository. The repository exists for two reasons:

- The MeCab website distributes MeCab and its dictionaries from a Google Drive
  folder, which download scripts cannot fetch easily.
- The release is encoded in EUC-JP. This repository provides the files in
  UTF-8.

Current version: **2.7.0-20250920**, the release 2.7.0-20070801 converted to
UTF-8.

## Versions

| Tag | Commit | Contents |
| --- | --- | --- |
| `2.7.0-20070801` | `0baf650` | The 52 files of the release, unmodified (EUC-JP) |
| `2.7.0-20250920` | `79f5146` | The same files, 33 of them converted to UTF-8 in [#1](https://github.com/lindera/mecab-ipadic/pull/1), and `convert_utf8.sh` |

## Contents

| Files | Contents | Origin |
| --- | --- | --- |
| `*.csv` (26 files) | Lexicon: the surface form, left and right context IDs, cost and features of each entry | Release, converted to UTF-8 |
| `matrix.def` | Connection costs between context IDs | Release |
| `left-id.def`, `right-id.def`, `pos-id.def` | Left and right context IDs, and part-of-speech IDs | Release, converted to UTF-8 |
| `char.def`, `unk.def` | Character categories, and entries for unknown words | Release, converted to UTF-8 |
| `feature.def`, `rewrite.def` | Feature templates and rewrite rules | Release, converted to UTF-8 |
| `dicrc` | Dictionary configuration for MeCab | Release |
| `Makefile.am`, `Makefile.in`, `configure`, `configure.in`, `aclocal.m4`, `config.guess`, `config.sub`, `install-sh`, `missing`, `mkinstalldirs` | Files for compiling and installing the dictionary with MeCab | Release |
| `RESULT` | Accuracy (precision, recall and F-measure) of this and earlier releases | Release |
| `AUTHORS`, `COPYING`, `INSTALL`, `README` | The authors, the license, installation instructions, and links to the former ChaSen and MeCab websites | Release |
| `ChangeLog`, `NEWS` | Empty files | Release |
| `convert_utf8.sh` | The script that converted the files to UTF-8 | Lindera |
| `README.md` | This file | Lindera |

## Sources

Checked on 2026-11-17.

The [download section](https://taku910.github.io/mecab/#download) of the MeCab
website links the IPA dictionary to the Google Drive folder
<https://drive.google.com/drive/folders/1-xcDvik4983YEEAOfkWhliDmarfqani_>.
That folder lists the releases 2.7.0-20051110, 2.7.0-20060707 and
2.7.0-20070610, but not 2.7.0-20070801. SourceForge, the former download site
of MeCab, serves the release 2.7.0-20070801:

| Where | URL |
| --- | --- |
| SourceForge | <https://sourceforge.net/projects/mecab/files/mecab-ipadic/2.7.0-20070801/mecab-ipadic-2.7.0-20070801.tar.gz/download> |

| Archive | Size | SHA-256 |
| --- | --- | --- |
| `mecab-ipadic-2.7.0-20070801.tar.gz` | 12,208,105 bytes | `b62f527d881c504576baed9c6ef6561554658b175ce6ae0096a60307e49e3523` |

The tarball holds 52 files under `mecab-ipadic-2.7.0-20070801/`. They are,
byte for byte, the files at the tag `2.7.0-20070801`. After the 33 files are
converted from EUC-JP to UTF-8 with `iconv`, they are, byte for byte, the files
of this repository other than `convert_utf8.sh` and `README.md`. To check,
clone this repository into `mecab-ipadic`, and run the following in the
directory that holds the clone:

```shell
curl -fsSL -o mecab-ipadic-2.7.0-20070801.tar.gz https://sourceforge.net/projects/mecab/files/mecab-ipadic/2.7.0-20070801/mecab-ipadic-2.7.0-20070801.tar.gz/download
echo 'b62f527d881c504576baed9c6ef6561554658b175ce6ae0096a60307e49e3523  mecab-ipadic-2.7.0-20070801.tar.gz' | sha256sum -c -
tar -xzf mecab-ipadic-2.7.0-20070801.tar.gz
cd mecab-ipadic-2.7.0-20070801
for f in *.csv char.def feature.def left-id.def pos-id.def rewrite.def right-id.def unk.def; do
  iconv -f EUC-JP -t UTF-8 "$f" > "$f.utf8" && mv "$f.utf8" "$f"
done
cd ..
diff -r -x .git mecab-ipadic-2.7.0-20070801 mecab-ipadic
```

The checksum matches, and `diff` names only the files that Lindera added:

```text
mecab-ipadic-2.7.0-20070801.tar.gz: OK
Only in mecab-ipadic: convert_utf8.sh
Only in mecab-ipadic: README.md
```

## Modifications

[#1](https://github.com/lindera/mecab-ipadic/pull/1) converted 33 files from
EUC-JP to UTF-8 with `convert_utf8.sh`: the 26 `*.csv` files, and `char.def`,
`feature.def`, `left-id.def`, `pos-id.def`, `rewrite.def`, `right-id.def` and
`unk.def`. The script tries Shift_JIS, EUC-JP and ISO-2022-JP in this order.
None of the files can be decoded as Shift_JIS, so every file was converted
from EUC-JP, and each converted file is the output of
`iconv -f EUC-JP -t UTF-8` for the file in the release.

The other 19 files of the release are unmodified:

- Except for `COPYING`, they hold only ASCII characters, or are empty.
- `COPYING` ends with two bytes, `0xF7 0xF7`, that are not a character in
  EUC-JP. They are kept as they are.
- `dicrc`, `Makefile.am` and `Makefile.in` still declare the input as EUC-JP
  (`config-charset = EUC-JP`, and `mecab-dict-index -f EUC-JP`), so they do not
  match the converted files. Lindera does not use them. Building the
  dictionary with MeCab from this repository has not been tested.

## Use in Lindera

`lindera-ipadic` downloads
<https://lindera.dev/mecab-ipadic-2.7.0-20250920.tar.gz> and checks its
SHA-256, `a7ba9f645ffe7094e56ae1c4a81d100df8fbb1e28bbe1792622e9728e162db3d`, in
[`lindera-ipadic/build.rs`](https://github.com/lindera/lindera/blob/main/lindera-ipadic/build.rs).
On 2026-11-17, that tarball was, byte for byte, the source archive that GitHub
generates for the tag `2.7.0-20250920`
(<https://github.com/lindera/mecab-ipadic/archive/refs/tags/2.7.0-20250920.tar.gz>).
[`lindera-ipadic/metadata.json`](https://github.com/lindera/lindera/blob/main/lindera-ipadic/metadata.json)
reads the files as UTF-8.

To build the dictionary, see
[Build](https://lindera.github.io/lindera/lindera-ipadic/build.html) in the
Lindera documentation.

## License

- mecab-ipadic is distributed under the license in `COPYING`, the same file as
  in the release. Its copyright holder is the Nara Institute of Science and
  Technology (NAIST), and its authors are listed in `AUTHORS`.
- `COPYING` requires every copy, in its original form or modified, to include
  its copyright notice and paragraphs. This repository keeps the file.

This repository is not affiliated with or endorsed by the authors of
mecab-ipadic.
