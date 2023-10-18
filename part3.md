

Q1. How to read file which contains 1 lacs records in nodejs

To read file with a large number of records (e.g., 1 lakh records) in Node.js efficiently, you should use a non-blocking and stream-based approach to avoid memory issues without loading the entire file into memory

Here's how you can do it using the fs and stream modules:

``` diff

const fs = require('fs');

const filename = 'large_file.txt'; // Replace with your file's name

const readStream = fs.createReadStream(filename, 'utf8');

let recordCount = 0;

readStream
  .on('data', (chunk) => {
    // Process the chunk of data (could be partial lines)
    const lines = chunk.split('\n');
    lines[0] = (recordCount > 0 ? '' : lines[0]) + lines[0];
    for (let i = 0; i < lines.length - 1; i++) {
      const line = lines[i];
      // Process each line here
      console.log(line);
      recordCount++;
    }
  })
  .on('end', () => {
    console.log('File reading complete.');
    console.log('Total records:', recordCount);
  })
  .on('error', (error) => {
    console.error('Error reading the file:', error);
  });

```

In this code:

We create a readable stream using fs.createReadStream. You should replace 'large_file.txt' with the actual file path that contains your records.

We set the encoding to 'utf8' to read the file as a text file.

Data

  We listen for the 'data' event, which is emitted when a new chunk of data is available for reading.
  Inside the 'data' event handler, we split the incoming chunk into lines based on newline characters (\n). We take care of any incomplete lines that may be split across chunks.
  We process each line as it becomes available and increment the recordCount.

end

  We listen for the 'end' event, which indicates that the file reading is complete. Here, you can perform any final tasks and display the total record count.

error 

  We also handle errors by listening for the 'error' event to ensure your code doesn't crash if any issues occur during file reading.

Using stream based approach allows nodejs to read and process the file in smaller manageable chunks, which is memroy-efficient and suitable for handling large files. 
This approach is important when working with files that contain a significant number of records.
