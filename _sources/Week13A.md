# Week 13a: MATLAB Scripts, Documentation, and File I/O 

## Learning Objectives
By the end of this prereading, you should be able to:
- Create and run MATLAB script files
- Document your code with comments and function headers
- Read data from and write data to text files
- Load and save MATLAB data files

## 1. MATLAB Scripts

### What is a Script?
A script is a file containing a sequence of MATLAB commands that execute together. Scripts are similar to Python `.py` files but use the `.m` extension in MATLAB.

### Creating a Script
To create a new script:
1. Click **New Script** in the MATLAB toolbar, or
2. Type `edit scriptname.m` in the command window

### Running a Script
There are three ways to run a script:
1. Click the **Run** button in the editor
2. Press **F5** while in the editor
3. Type the script name (without `.m`) in the command window

### Key Differences from Python
- MATLAB scripts share the workspace with the command window (no explicit `if __name__ == "__main__"` needed)
- All variables created in a script remain in the workspace after execution
- Scripts don't require special syntax to run; just save and execute

### Example Script
```matlab
% data_analysis.m
% This script analyzes temperature data

temperatures = [72, 75, 78, 73, 71, 74, 76];
avg_temp = mean(temperatures);
max_temp = max(temperatures);

fprintf('Average temperature: %.2f°F\n', avg_temp);
fprintf('Maximum temperature: %.2f°F\n', max_temp);
```

## 2. Documenting Your Code

### Single-Line Comments
Use the `%` symbol for comments (similar to `#` in Python):
```matlab
% This is a comment
x = 5;  % Inline comment
```

### Multi-Line Comments (Block Comments)
Use `%{` and `%}` for block comments:
```matlab
%{
This is a multi-line comment.
It can span several lines.
Useful for longer explanations.
%}
```

### Header Documentation
Always include a header at the top of your scripts with:
- Purpose of the script
- Author and date
- Input requirements
- Output description

```matlab
% ANALYZE_GRADES - Calculate statistics for student grades
%
% Author: Your Name
% Date: November 2025
%
% Description:
%   This script reads grade data, calculates mean and median,
%   and generates a histogram of the distribution.
%
% Inputs: grades.txt (text file with one grade per line)
% Outputs: Grade statistics printed to console and histogram figure
```

### The `help` Command
When you type `help scriptname`, MATLAB displays the first contiguous comment block. This makes good header documentation essential for code reusability.

## 3. Importing Data from Different File Types

MATLAB can work with many file formats. Choosing the right import function depends on your data structure and file type.

### CSV Files (Comma-Separated Values)

CSV files are common for spreadsheet data and data exchange between programs.

#### Using `readmatrix` (Numeric Data Only)
```matlab
data = readmatrix('grades.csv');  % Returns numeric array
```

#### Using `readtable` (Mixed Data Types - Recommended)
```matlab
dataTable = readtable('students.csv');  % Returns table with column names
% Access columns by name
names = dataTable.Name;
scores = dataTable.Score;
```

#### Handling Headers and Options
```matlab
% Skip header rows, specify delimiter
data = readmatrix('data.csv', 'NumHeaderLines', 1);

% Read table with specific options
opts = detectImportOptions('data.csv');
opts.VariableTypes(3) = {'double'};  % Force column 3 to be numeric
dataTable = readtable('data.csv', opts);
```

### Excel Files (.xlsx, .xls)

Excel files can contain multiple sheets and mixed data types.

#### Reading Excel Data
```matlab
% Read numeric data from first sheet
data = readmatrix('experiment.xlsx');

% Read as table (preserves headers and data types)
dataTable = readtable('experiment.xlsx');

% Read specific sheet
dataTable = readtable('experiment.xlsx', 'Sheet', 'Results');

% Read specific range
data = readmatrix('experiment.xlsx', 'Range', 'B2:D10');
```

#### Writing Excel Data
```matlab
% Write matrix to Excel
results = [1, 2, 3; 4, 5, 6];
writematrix(results, 'output.xlsx');

% Write table to Excel (includes headers)
T = table([1; 2; 3], [4; 5; 6], 'VariableNames', {'A', 'B'});
writetable(T, 'output.xlsx');
```

### Text Files with Delimiters

For tab-delimited or other custom formats:

```matlab
% Tab-delimited file
data = readmatrix('data.txt', 'Delimiter', '\t');

% Space-delimited file
data = readmatrix('data.txt', 'Delimiter', ' ');

% Custom delimiter
data = readmatrix('data.txt', 'Delimiter', '|');
```

### Image Files

MATLAB can read and write various image formats and handles color images as 3D arrays.

#### Understanding Image Data Structure
```matlab
% Read image
img = imread('photo.jpg');  % Works with .jpg, .png, .tif, etc.

% Check image dimensions
size(img)  % Returns [height, width, channels]
% Grayscale: [rows, cols] - 2D array
% Color (RGB): [rows, cols, 3] - 3D array with red, green, blue layers
```

#### Working with Color Images
Color images in MATLAB are stored as 3D arrays where:
- 1st dimension = height (rows)
- 2nd dimension = width (columns)
- 3rd dimension = color channels (3 layers: Red, Green, Blue)

```matlab
% Read a color image
colorImg = imread('sunset.jpg');

% Access individual color channels
redChannel = colorImg(:, :, 1);    % All red values
greenChannel = colorImg(:, :, 2);  % All green values
blueChannel = colorImg(:, :, 3);   % All blue values

% Display original and channels
figure;
subplot(2,2,1); imshow(colorImg); title('Original');
subplot(2,2,2); imshow(redChannel); title('Red Channel');
subplot(2,2,3); imshow(greenChannel); title('Green Channel');
subplot(2,2,4); imshow(blueChannel); title('Blue Channel');
```

#### Converting Between Color and Grayscale
```matlab
% Convert color to grayscale
grayImg = rgb2gray(colorImg);

% Convert grayscale to "color" (3 identical channels)
colorFromGray = cat(3, grayImg, grayImg, grayImg);
```

#### Creating Modified Color Images
```matlab
% Create a copy and modify specific channel
modifiedImg = colorImg;
modifiedImg(:, :, 1) = modifiedImg(:, :, 1) * 1.5;  % Boost red channel

% Set a channel to zero (remove that color)
noRed = colorImg;
noRed(:, :, 1) = 0;  % Remove all red
```

#### Image Data Types
```matlab
% Most images are uint8 (0-255)
class(img)  % Usually returns 'uint8'

% Convert to double for calculations (0.0-1.0 range)
imgDouble = im2double(img);

% Convert back to uint8 for display/saving
imgUint8 = im2uint8(imgDouble);
```

#### Saving Images
```matlab
% Write image
imwrite(colorImg, 'output.png');

% Write with quality setting (for JPEG)
imwrite(colorImg, 'output.jpg', 'Quality', 95);
```

## 4. Saving Data for Reuse

## 8. Saving Data for Reuse

`.mat` files are MATLAB's native format and the most efficient way to save workspace data.

#### Advantages of .mat Files
- Fast to read and write
- Preserves variable names and data types
- Can store any MATLAB data type (matrices, tables, structures, cell arrays)
- Compressed by default to save space

#### Saving to .mat Files
```matlab
% Save specific variables
temperature = [72, 75, 78, 73];
pressure = [1013, 1015, 1012, 1014];
save('experiment_data.mat', 'temperature', 'pressure');

% Save all workspace variables
save('my_workspace.mat');

% Save with version compatibility (for older MATLAB versions)
save('data.mat', 'x', 'y', '-v7.3');
```

#### Loading from .mat Files
```matlab
% Load all variables from file
load('experiment_data.mat');  % Variables appear in workspace

% Load specific variables only
load('experiment_data.mat', 'temperature');

% Load into a structure (avoids overwriting existing variables)
data = load('experiment_data.mat');
temp = data.temperature;
```

### CSV Files - Best for Data Exchange

Use CSV when sharing data with other programs (Excel, Python, R) or for human-readable output.

#### Writing CSV Files
```matlab
% Write matrix to CSV
data = [1, 2, 3; 4, 5, 6; 7, 8, 9];
writematrix(data, 'results.csv');

% Write table to CSV (includes column headers)
T = table([10; 20; 30], [40; 50; 60], 'VariableNames', {'Time', 'Value'});
writetable(T, 'results.csv');
```

### Excel Files - Best for Reports and Sharing

Excel files are ideal when you need formatting or want non-technical users to view your data.

```matlab
% Write to specific sheet
writematrix(data, 'report.xlsx', 'Sheet', 'Raw Data');
writematrix(summary, 'report.xlsx', 'Sheet', 'Summary');

% Write table with headers
writetable(dataTable, 'report.xlsx', 'Sheet', 'Results');
```

### Text Files - For Custom Formats

Use `fprintf` when you need complete control over formatting:

```matlab
fileID = fopen('report.txt', 'w');
fprintf(fileID, '=== Experimental Results ===\n\n');
fprintf(fileID, 'Average: %.2f\n', mean(data));
fprintf(fileID, 'Std Dev: %.2f\n', std(data));
fclose(fileID);
```

### Choosing the Right Format

| Format | Use When | Advantages | Disadvantages |
|--------|----------|------------|---------------|
| .mat | Working within MATLAB | Fast, preserves types | MATLAB-specific |
| .csv | Sharing with other programs | Universal, readable | Numeric/text only |
| .xlsx | Creating reports | Formatted, familiar | Slower, requires Excel |
| .txt | Custom formatting needed | Full control | Manual parsing |

## 5. Working with Directories and File Paths

Understanding how to navigate directories and manage file paths is essential for organizing your projects and working with data files.

### Current Directory Operations

#### Checking and Changing Directories
```matlab
% Show current directory
currentDir = pwd

% Change directory (absolute path)
cd('C:\Users\YourName\Documents\MATLAB')

% Change to parent directory
cd ..

% Change to subdirectory
cd('Data')

% Return to previous directory
cd(currentDir)
```

#### Listing Directory Contents
```matlab
% List all files and folders in current directory
dir

% List specific file types
dir('*.csv')  % All CSV files
dir('*.mat')  % All MAT files
dir('Data/*.txt')  % All text files in Data subdirectory

% Store directory listing in variable
files = dir('*.csv');
% Access properties
firstFile = files(1).name;  % Name of first file
fileSize = files(1).bytes;  % Size in bytes
```

### Building File Paths

#### The fullfile Function (Recommended)
Using `fullfile` ensures your code works across different operating systems (Windows, Mac, Linux):

```matlab
% Build path to file in subdirectory
folder = 'Data';
filename = 'experiment1.csv';
filepath = fullfile(folder, filename);
% Result: 'Data\experiment1.csv' (Windows) or 'Data/experiment1.csv' (Mac/Linux)

% Build nested directory path
basePath = 'C:\Users\YourName\Documents';
project = 'MATLAB_Project';
dataFolder = 'Data';
file = 'results.mat';
fullPath = fullfile(basePath, project, dataFolder, file);

% Use in file operations
data = load(fullPath);
```

#### Path Separators
```matlab
% Windows uses backslash
windowsPath = 'C:\Users\Data\file.csv';

% Mac/Linux use forward slash
unixPath = '/home/user/Data/file.csv';

% filesep gives the correct separator for your system
separator = filesep;  % '\' on Windows, '/' on Mac/Linux

% Manual path building (not recommended)
manualPath = ['Data', filesep, 'results.csv'];
```

### Relative vs Absolute Paths

#### Absolute Paths
Full path from the root of your file system:
```matlab
% Windows absolute path
data = readmatrix('C:\Users\YourName\Documents\data.csv');

% Mac/Linux absolute path
data = readmatrix('/home/username/Documents/data.csv');
```

#### Relative Paths
Path relative to your current directory:
```matlab
% File in current directory
data = readmatrix('data.csv');

% File in subdirectory
data = readmatrix('Data\experiment1.csv');
% Or better (cross-platform):
data = readmatrix(fullfile('Data', 'experiment1.csv'));

% File in parent directory
data = readmatrix('..\data.csv');
% Or better:
data = readmatrix(fullfile('..', 'data.csv'));

% File two levels up
data = readmatrix(fullfile('..', '..', 'shared_data.csv'));
```

### Creating and Managing Directories

#### Creating Directories
```matlab
% Create single directory
mkdir('Results')

% Create nested directories
mkdir('Data\Processed')
% Or cross-platform:
mkdir(fullfile('Data', 'Processed'))

% Check if directory exists before creating
if ~isfolder('Results')
    mkdir('Results')
end
```

#### Checking Existence
```matlab
% Check if file exists
if isfile('data.csv')
    data = readmatrix('data.csv');
else
    error('File not found!');
end

% Check if directory exists
if isfolder('Data')
    cd('Data')
else
    mkdir('Data')
end
```

#### Deleting Files and Directories
```matlab
% Delete a file
delete('old_results.txt')

% Delete directory (must be empty)
rmdir('OldData')

% Delete directory and all contents
rmdir('OldData', 's')  % 's' flag for recursive deletion
```

### Working with Multiple Files

#### Processing All Files in a Directory
```matlab
% Get list of all CSV files
fileList = dir('*.csv');

% Loop through each file
for i = 1:length(fileList)
    filename = fileList(i).name;
    filepath = fullfile(fileList(i).folder, filename);
    
    % Read and process each file
    data = readmatrix(filepath);
    avgValue = mean(data, 'all');
    
    fprintf('File: %s, Average: %.2f\n', filename, avgValue);
end
```

#### Pattern Matching with dir
```matlab
% Find all files starting with "exp"
files = dir('exp*.csv');

% Find all files containing "2024"
files = dir('*2024*.txt');

% Find files in subdirectories (R2016b and later)
files = dir('**/*.csv');  % Recursive search for all CSV files
```

### Organizing Project Files

#### Recommended Directory Structure
```
MyProject/
├── Data/
│   ├── Raw/
│   └── Processed/
├── Scripts/
├── Results/
│   ├── Figures/
│   └── Tables/
└── Documentation/
```

#### Example: Saving Results to Organized Folders
```matlab
% Set up project structure
projectRoot = pwd;
resultsFolder = fullfile(projectRoot, 'Results');
figuresFolder = fullfile(resultsFolder, 'Figures');

% Create folders if they don't exist
if ~isfolder(resultsFolder)
    mkdir(resultsFolder);
end
if ~isfolder(figuresFolder)
    mkdir(figuresFolder);
end

% Save figure to organized location
plot(x, y);
saveas(gcf, fullfile(figuresFolder, 'plot1.png'));

% Save data to organized location
save(fullfile(resultsFolder, 'analysis_results.mat'), 'results');
```

### Getting File Information

```matlab
% Get detailed file information
fileInfo = dir('data.csv');
fileName = fileInfo.name;
fileSize = fileInfo.bytes;
dateModified = fileInfo.date;

fprintf('File: %s\n', fileName);
fprintf('Size: %d bytes\n', fileSize);
fprintf('Modified: %s\n', dateModified);

% Extract parts of file path
[filepath, name, ext] = fileparts('Data\experiment1.csv');
% filepath = 'Data'
% name = 'experiment1'
% ext = '.csv'
```

### Best Practices for File Paths

1. **Use `fullfile` for portability** - Your code will work on any operating system
2. **Use relative paths when possible** - Makes projects easier to share
3. **Check for existence before operations** - Use `isfile` and `isfolder`
4. **Organize with subdirectories** - Separate raw data, processed data, and results
5. **Use meaningful directory names** - Clear structure helps with project management
6. **Avoid spaces in file/folder names** - Use underscores instead: `my_data.csv` not `my data.csv`

### Common Pitfalls

```matlab
% DON'T: Hardcode full paths
data = load('C:\Users\John\Documents\MATLAB\data.csv');  % Won't work on other computers

% DO: Use relative paths
data = load(fullfile('Data', 'data.csv'));

% DON'T: Use system-specific separators
path = 'Data\Raw\experiment.csv';  % Breaks on Mac/Linux

% DO: Use fullfile
path = fullfile('Data', 'Raw', 'experiment.csv');
```

## 7. File Input/Output - Basic Operations

### Reading Text Files

#### Method 1: `load` for Simple Numeric Data
The `load` command reads numeric data from text files:
```matlab
data = load('numbers.txt');  % Creates variable with file data
```

#### Method 2: `readmatrix` (Recommended for Modern MATLAB)
```matlab
data = readmatrix('datafile.txt');  % More flexible than load
```

#### Method 3: Reading Line by Line with `fopen`, `fgetl`, `fclose`
For more control:
```matlab
fileID = fopen('data.txt', 'r');  % 'r' means read mode
line = fgetl(fileID);  % Get one line
fclose(fileID);  % Always close files!
```

#### Reading All Lines from a File
```matlab
fileID = fopen('data.txt', 'r');
data = [];
while ~feof(fileID)  % while not end-of-file
    line = fgetl(fileID);
    value = str2double(line);  % Convert string to number
    data = [data; value];
end
fclose(fileID);
```

### Writing Text Files

#### Method 1: `save` for Simple Numeric Data
```matlab
results = [1, 2, 3; 4, 5, 6];
save('results.txt', 'results', '-ascii');
```

#### Method 2: `fprintf` for Formatted Output
```matlab
fileID = fopen('output.txt', 'w');  % 'w' means write mode
fprintf(fileID, 'Temperature: %.2f\n', temp);
fprintf(fileID, 'Pressure: %.2f\n', pressure);
fclose(fileID);
```

Common format specifiers:
- `%d` - integer
- `%f` - floating point
- `%.2f` - floating point with 2 decimal places
- `%s` - string
- `\n` - newline

## 9. File Management Best Practices

### Check if Files Exist
```matlab
if isfile('data.txt')
    data = load('data.txt');
else
    error('File not found!');
end
```

### File Modes
When using `fopen`, remember these modes:
- `'r'` - read only
- `'w'` - write (overwrites existing file)
- `'a'` - append (adds to end of existing file)

### Always Close Files
Forgetting to close files can cause problems. Always use `fclose(fileID)` after opening files with `fopen`.

