a











## proc目录

- 这个目录下存储了进程的所有信息
  - ​	**通过进程的PID来找到当前进程说对应的目录**
    - **找到后的进程目录内容:**
      - **`cmdline`** : 该文件存储了进程的包名,例如 `com.tencent.tmgp.pubgmhd`
      - **`maps`** : 记录了当前进程的动态库加载信息



### 通过进程包名获得进程 PID

```c++
// 通过 进程包名 获得进程 PID  , 例如 包名为 com.tencent.tmgp.pubgmhd

int getPID(const char* PackageName)  // PackageName = com.tencent.tmgp.pubgmhd
{
	DIR* dir = NULL;
	struct dirent* ptr = NULL;
	FILE* fp = NULL;
	char filepath[256];
	char filetext[128];
	dir = opendir("/proc");		// 打开路径
	if (NULL != dir)
	{
		while ((ptr = readdir(dir)) != NULL)
		{
			if ((strcmp(ptr->d_name, ".") == 0) || (strcmp(ptr->d_name, "..") == 0))
				continue;
			if (ptr->d_type != DT_DIR)
				continue;
			//只寻找目录
			sprintf(filepath, "/proc/%s/cmdline", ptr->d_name);
			fp = fopen(filepath, "r");
			if (NULL != fp)
			{
				fgets(filetext, sizeof(filetext), fp);
				if (strcmp(filetext, PackageName) == 0)
				{
					break;
				}
				fclose(fp);
			}
		}
	}
	if (readdir(dir) == NULL)
	{
		return 0;
	}
	closedir(dir);
	return atoi(ptr->d_name);
}
```



### 获得动态库载入的位置地址

```c++
// str 是动态库名  libUE4.so 
unsigned long GetModuaddr(const char* str)
{
	char line[1024];
	unsigned long start, end;
	char maps[1024];
	sprintf(maps, "/proc/%d/maps", pid);
	FILE* f_p = fopen(maps, "r");
	while (fgets(line, sizeof(line), f_p) != NULL)
	{
		if (strstr(line, "r") != NULL && strstr(line, str))
		{
			sscanf(line, "%lx-%lx", &start, &end);
			return start;
		}
	}
	fclose(f_p);
	return 0;
}
```





