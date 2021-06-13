# soal-shift-sisop-modul-4-I06-2021
## Group Member Name:
1. Khairi Wiryawan                      (05111942000023)
2. Juan Carlos Tepanus Pardosi          (05111942000017)
3. Hilmy Hanif Ibrahim					        (05111942000005)

## Problem 1
In a department, there was a new lab admin who had nothing to do, his name was Sin. Sin just became an admin exactly 1 month ago. After a month in that lab he finally met great people, one of them was Sei. Sei and Sin became good friends. Because in the past few months there was a lot of buzz about data breach, they decided to make a file system with a robust encode method technique. The file system is as follows :

NOTE : 
Every file on the directory must be encoded using menggunakan Atbash cipher(mirror).
For example, for a file named kucinglucu123.jpg on DATA_PENTING directory
“AtoZ_folder/DATA_PENTING/kucinglucu123.jpg” → “AtoZ_folder/WZGZ_KVMGRMT/pfxrmtofxf123.jpg”
Note : the file system works normally like any linux file system, Mount source (root) of the file system is on /home/[USER]/Downloads directory, when naming a file,  ‘/’ is ignored, and extension of a file doesn’t need to be encoded.
### 1A 
If a directory starts with “AtoZ_”, then that directory will be encoded.
### 1B
If you rename a directory to have a prefix “AtoZ_”, then the directory will be encoded.
### 1C
When encoded directory is renamed to not have encoded name, then the directory will be decoded 
### 1D
For every encoding of a directory (mkdir or rename) will be recorded in a log file. The format is : /home/[USER]/Downloads/[Directory Name] → /home/[USER]/Downloads/AtoZ_[Directory Name]
### 1E
Encoding method is also applied to all other directories inside the encoded directory.(rekursif)

## Problem 2
Other than that, Sei proposed to create additional encryption methods to increase the security of their computer data . The following is the additional encryption method designed by Sei

### 2A
If a directory is created starting with “RX_[Nama]”, then that directory and its contents will be encoded with a rename according to problem 1 with an additional ROT13 algorithm (Atbash + ROT13).
### 2B
If a directory is renamed starting with “RX_[Nama]”, then that directory and its contents will be encoded with a rename according to problem 1 with an additional Vigenere Cipher algorithm with “SISOP” as it's key (Case-sensitive, Atbash + Vigenere).
### 2C
If an encoded directory is renamed (Removing the “RX_”), then that folder will become unencoded and it's directory contents will be decoded based on it's real name.
### 2D
Every encoded directory created (mkdir or rename) will be noted to a log file with it's methods (whether it's mkdir or rename).
### 2E
For this encryption method, files in the original directory will be split into smaller, 1024 byte files. While if accessed via the file system designed by Sin and Sei, files will become normal. Example, Suatu_File.txt sized 3 kiloBytes in its original directory will become 3 smaller files::

Suatu_File.txt.0000
Suatu_File.txt.0001
Suatu_File.txt.0002

When accessed via the file system, file will appear as Suatu_File.txt

## Problem 3
Because Sin still feels exceptionally empty, he finally adds another feature to their file system.
### 3A
If a directory is created with the prefix "A_is_a_", it will become a special directory.
### 3B
If a directory is renamed with the prefix "A_is_a_", it will become a special directory.
### 3C
If the encrypted directory is renamed by deleting "A_is_a_" at the beginning of the folder name, that directory becomes a normal directory.
### 3D
Special directories are directories that return encryption/encoding to the "AtoZ_" or "RX_" directories but their respective rules still run in the directory within them ("AtoZ_" and "RX_" recursive properties still run in subdirectories).
### 3E
In the special directory, all filenames (excluding extensions) in the fuse will be changed to lowercase (insensitive) and will be given a new extension in the form of a decimal value from the binary value that comes from the difference in character between filenames.


For example, if in the original directory the filename is "FiLe_CoNtoH.txt" then in the fuse it will be "file_contoh.txt.1321". 1321 comes from binary 10100101001.

## Problem 4
To make it easier to monitor activities on their filesystem, Sin and Sei created a log system with the following specifications.
### 4A
The system log that will be created is named “SinSeiFS.log” in the user's home directory (/home/[user]/SinSeiFS.log). This system log maintains a list of system call commands that have been executed on the filesystem.
### 4B
Because Sin and Sei like tidiness, the logs that are made will be divided into two levels,INFO and WARNING.
### 4C
For the WARNING level log, it is used to log the rmdir and unlink syscalls.
### 4D
The rest will be recorded at the INFO level.
### 4E
The format for logging is:


[Level]::[dd][mm][yyyy]-[HH]:[MM]:[SS]:[CMD]::[DESC :: DESC]

Level : Level logging, dd : 2 digit date, mm : 2 digit month, yyyy : 4 digit year, HH : 2 digit hour (24 Hour format),MM : 2 digit minute, SS : 2 digit second, CMD : Called System Call, DESC : additional information and parameters

INFO::28052021-10:00:00:CREATE::/test.txt
INFO::28052021-10:01:00:RENAME::/test.txt::/rename.txt

### Answer
so, we answer the question of 1, 2(except e), and 4

these functions:
```
void tulisLog(char *nama, char *fpath)
{
	time_t rawtime;
	struct tm *timeinfo;
	time(&rawtime);
	timeinfo = localtime(&rawtime);

	char haha[1000];

	FILE *file;
	file = fopen("/home/asusry/SinSeiFS.log", "a");

	if (strcmp(nama, "RMDIR") == 0 || strcmp(nama, "UNLINK") == 0)
		sprintf(haha, "WARNING::%.2d%.2d%d-%.2d:%.2d:%.2d::%s::%s\n", timeinfo->tm_mday, timeinfo->tm_mon + 1, timeinfo->tm_year + 1900, timeinfo->tm_hour, timeinfo->tm_min, timeinfo->tm_sec, nama, fpath);
	else
		sprintf(haha, "INFO::%.2d%.2d%d-%.2d:%.2d:%.2d::%s::%s\n", timeinfo->tm_mday, timeinfo->tm_mon + 1, timeinfo->tm_year + 1900, timeinfo->tm_hour, timeinfo->tm_min, timeinfo->tm_sec, nama, fpath);

	fputs(haha, file);
	fclose(file);
	return;
}

void tulisLog2(char *nama, const char *from, const char *to)
{
	time_t rawtime;
	struct tm *timeinfo;
	time(&rawtime);
	timeinfo = localtime(&rawtime);

	char haha[1000];

	FILE *file;
	file = fopen("/home/asusry/SinSeiFS.log", "a");

	if (strcmp(nama, "RMDIR") == 0 || strcmp(nama, "UNLINK") == 0)
		sprintf(haha, "WARNING::%.2d%.2d%d-%.2d:%.2d:%.2d::%s::%s::%s\n", timeinfo->tm_mday, timeinfo->tm_mon + 1, timeinfo->tm_year + 1900, timeinfo->tm_hour, timeinfo->tm_min, timeinfo->tm_sec, nama, from, to);
	else
		sprintf(haha, "INFO::%.2d%.2d%d-%.2d:%.2d:%.2d::%s::%s::%s\n", timeinfo->tm_mday, timeinfo->tm_mon + 1, timeinfo->tm_year + 1900, timeinfo->tm_hour, timeinfo->tm_min, timeinfo->tm_sec, nama, from, to);

	fputs(haha, file);
	fclose(file);
	return;
}
```
is dedicated to answer the question 4 regarding writing a log.

No 1 and 2 require an Atbash encryption and decryption. here are the functions of the Atbash encryption and decryption.
```
void encryptAtbash(char *path)
{
	if (!strcmp(path, ".") || !strcmp(path, "..")) return;
	
	printf("encrypt path Atbash: %s\n", path);
	
	int endid = split_ext_id(path);
	if(endid == strlen(path)) endid = ext_id(path);
	int startid = slash_id(path, 0);
	
	for (int i = startid; i < endid; i++){
		if (path[i] != '/' && isalpha(path[i])){
			char tmp = path[i];
			if(isupper(path[i])) tmp -= 'A';
			else tmp -= 'a';
			tmp = 25 - tmp; //Atbash cipher
			if(isupper(path[i])) tmp += 'A';
			else tmp += 'a';
			path[i] = tmp;
		}
	}
}

void decryptAtbash(char *path)
{
	if (!strcmp(path, ".") || !strcmp(path, "..")) return;
	
	printf("decrypt path Atbash: %s\n", path);
	
	int endid = split_ext_id(path);
	if(endid == strlen(path)) endid = ext_id(path);
	int startid = slash_id(path, endid);
	
	for (int i = startid; i < endid; i++){
		if (path[i] != '/' && isalpha(path[i])){
			char tmp = path[i];
			if(isupper(path[i])) tmp -= 'A';
			else tmp -= 'a';
			tmp = 25 - tmp; //Atbash cipher
			if(isupper(path[i])) tmp += 'A';
			else tmp += 'a';
			path[i] = tmp;
		}
	}
}
```

for the question no 2, we need additional algorithm which is ROT13. here are the functions for ROT13
```
void encryptRot13(char *path)
{
	if (!strcmp(path, ".") || !strcmp(path, "..")) return;
	
	printf("encrypt path ROT13: %s\n", path);
	
	int endid = split_ext_id(path);
	int startid = slash_id(path, 0);
	
	for (int i = startid; i < endid; i++){
		if (path[i] != '/' && isalpha(path[i])){
			char tmp = path[i];
			if(isupper(path[i])) tmp -= 'A';
			else tmp -= 'a';
			tmp = (tmp + 13) % 26; //ROT13 cipher
			if(isupper(path[i])) tmp += 'A';
			else tmp += 'a';
			path[i] = tmp;
		}
	}
}

void decryptRot13(char *path)
{
	if (!strcmp(path, ".") || !strcmp(path, "..")) return;
	
	printf("decrypt path ROT13: %s\n", path);
	
	int endid = split_ext_id(path);
	int startid = slash_id(path, endid);
	
	for (int i = startid; i < endid; i++){
		if (path[i] != '/' && isalpha(path[i])){
			char tmp = path[i];
			if(isupper(path[i])) tmp -= 'A';
			else tmp -= 'a';
			tmp = (tmp + 13) % 26; //ROT13 cipher
			if(isupper(path[i])) tmp += 'A';
			else tmp += 'a';
			path[i] = tmp;
		}
	}
}
```
and also, no 2 need Vigenere algorithm.
```
void encryptVigenere(char *path)
{
	if (!strcmp(path, ".") || !strcmp(path, "..")) return;
	
	printf("encrypt path Vigenere: %s\n", path);
	
	int endid = split_ext_id(path);
	int startid = slash_id(path, 0);
	
	for (int i = startid; i < endid; i++){
		if (path[i] != '/' && isalpha(path[i])){
			char tmp1 = path[i];
			char tmp2 = key[(i-startid) % strlen(key)];
			if(isupper(path[i])){
				tmp1 -= 'A';
				tmp2 -= 'A';
				tmp1 = (tmp1 + tmp2) % 26; //Vigenere cipher
				tmp1 += 'A';
			}
			else{
				tmp1 -= 'a';
				tmp2 = tolower(tmp2) - 'a';
				tmp1 = (tmp1 + tmp2) % 26; //Vigenere cipher
				tmp1 += 'a';
			}
			path[i] = tmp1;
		}
	}
}

void decryptVigenere(char *path)
{
	if (!strcmp(path, ".") || !strcmp(path, "..")) return;
	
	printf("decrypt path Vigenere: %s\n", path);
	
	int endid = split_ext_id(path);
	int startid = slash_id(path, endid);
	
	for (int i = startid; i < endid; i++){
		if (path[i] != '/' && isalpha(path[i])){
			char tmp1 = path[i];
			char tmp2 = key[(i-startid) % strlen(key)];
			if(isupper(path[i])){
				tmp1 -= 'A';
				tmp2 -= 'A';
				tmp1 = (tmp1 - tmp2 + 26) % 26; //Vigenere cipher
				tmp1 += 'A';
			}
			else{
				tmp1 -= 'a';
				tmp2 = tolower(tmp2) - 'a';
				tmp1 = (tmp1 - tmp2 + 26) % 26; //Vigenere cipher
				tmp1 += 'a';
			}
			path[i] = tmp1;
		}
	}
}
```

### Screenshot
