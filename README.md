# get_next_line
`get_next_line()` is a C function designed to read a single line from a file descriptor. This project demonstrates an efficient way to process file input line by line, which can be especially useful for scenarios like parsing configuration files, processing logs, or handling standard input in real-time applications.

##Key Features
- Reads one line at a time from a given file descriptor.
- Handles multiple file descriptors simultaneously.
- Flexible buffer size via **BUFFER_SIZE** macro.
- Returns **NULL** on EOF or error for seamless integration into larger projects.

## Functionality
`char *get_next_line(int fd)`

The function takes as parameter a *file descriptor* previously opened to read into a *static char *buffer*. The buffer needs to be **static** in the case that when you read from the file, 

```
char	*get_next_line(int fd)
{
	static char	*buffer = NULL;
	char		*line;

	if (fd < 0 || read(fd, NULL, 0) == -1 || BUFFER_SIZE <= 0)
	{
		if (buffer != NULL)
		{
			free(buffer);
			buffer = NULL;
		}
		return (NULL);
	}
	if (buffer == NULL || !ft_strchr(buffer, '\n'))
		buffer = read_from_fd(&buffer, fd);
	if (buffer == NULL)
		return (NULL);
	line = extract_line(buffer);
	if (line == NULL)
	{
		free(buffer);
		return (buffer = NULL, line);
	}
	buffer = obtain_remain(&buffer);
	return (line);
}
```

//why a static char buffer?
