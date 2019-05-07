# Part 2: Intro to Linux

***Basic introduction to Linux***

To obtain the current directory that the shell is in, type in the open shell terminal:

```bash
pwd
```

 To determine the current active account:

```bash
whoami
```

To see a listing of all subdirectories and files in the current directory in a long list form, type:

```
ls -l
```

To see the hidden files as well:

```
ls -la
```

To navigate to the parent directory, type:

```
cd ..
pwd
```

To create directory:

```
mkdir <directory name>
```

To create nested directory:

```
mkdir -p <1st lvl dir/ 2nd lvl dir/ ...>
```

To remove directory of file:

```
rm <directory / file>
```

To remove nested directory with prompt:

```
rm -r <1st lvl dir>
```

To remove nested directory without prompt:

```
rm -rf <1st lvl dir>
```

Create file with self-defined extension:

```
touch <filename>.<extension name>
```

Create and edit file in CentOS shell:

```
gedit example.txt
```

Edit file in shell:

```
vi example.txt
```

Show file content:

```
less example.txt
cat example.txt
```

Make a copy of the single file in the directory:

```
cp file1.txt file1copy.txt
ls -l
```

Make a copy of the single file in other directory:

```
cp file1.txt ../../
cp file1.txt ~
```

Move files to other directory:

```
mv file1.txt ../../
```

Rename files:

```
mv file1.txt example.txt 
```

Using wildcard:

```
rm -f *.txt
rm -f ?xamp?.txt
mv *.txt newdirectory
```

