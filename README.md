# UpdateNet6FileUploadReadAsyncIssue
Update Blazor project to Net 6 from Net 5, IBrowerFile's ReadAsync cannot read all bytes (reported https://github.com/dotnet/aspnetcore/issues/38842).
.Net 6 ReadAsync function reads the random number of data and not like in .Net 5, specific amount,
a workaround code for .NET 6:

            var buf = new byte[file.Size];
            using var readStream = file.OpenReadStream(maxFileSize);
            var bytesRead = 0;
            var totalRead = 0;
            var buffer = new byte[1024 * 10];

            while ((bytesRead = await readStream.ReadAsync(buffer)) != 0)
            {
                Array.Copy(buffer, 0, buf, totalRead, bytesRead);
                totalRead += bytesRead;
            }
