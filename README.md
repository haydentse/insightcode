# insightcode


 #include <iostream>
 #include <fstream>
 #include <string>
 #include <map> 
 #include <sstream> 

using namespace std;

int main() {
	ifstream myInfile;
	myInfile.open("tweet_input.txt");
	string word, line;
	map<string, int> mymap;
	int num_of_lines = 0;
	double linesize[256]; 
	int i = 0;
	int x, y, temp;

	if (!myInfile) 
	{
		   
		cout << "Error\n\n";
		return 1;
	}

	// feature 2
	ofstream feature2("ft2.txt");
	while (!myInfile.eof())
	{
		num_of_lines++;
		mymap.clear();
		getline(myInfile, line);
		if (!line.empty()) 
		{
			istringstream iss(line, istringstream::in);
			while (iss >> word) 
			{
				mymap[word]++;
			}
			linesize[num_of_lines - 1] = mymap.size(); 

			
			for (x = 0; x < num_of_lines - 1; x++){
				for (y = 0; y < num_of_lines - 1; y++) {
					if (linesize[x] < linesize[y]) {
						temp = linesize[x];
						linesize[x] = linesize[y];
						linesize[y] = temp;
					}
				}
			}
			if (num_of_lines % 2 == 1) 
				feature2 << linesize[num_of_lines / 2] << endl;
			else
				feature2 << (linesize[num_of_lines / 2] + linesize[(num_of_lines/ 2) - 1]) / 2 << endl;	
		}
	}

	myInfile.clear();
	myInfile.seekg(0, ios::beg);
	mymap.clear();  //Clear the map for feature 1

	// feature 1
	while (!myInfile.eof()) {
		myInfile >> word;
		mymap[word]++;
	}
	ofstream feature1("ft1.txt");
	for (map<string, int>::iterator it = mymap.begin(); it != mymap.end(); ++it) {
		feature1.width(35);
		feature1 << left << it->first << right << it->second << '\n';
	}
	myInfile.close(); // Close files
	return 0;
}

