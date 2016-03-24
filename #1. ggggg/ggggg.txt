#include <string>
#include <iostream>
#include <fstream>
#include <vector>
using namespace std;

class GGG
{
	vector<string> key;
	vector<char> code;
	vector<char> text;
public:
	GGG()
	{
		key.reserve(54);
		char a = 150;
		for (int i = 0; i < 54; i++)
		{
			key.push_back("x");
		}
	}
	bool encode()
	{
		ofstream writer("encoded.txt");
		for (int i = 0; i < key.size(); i++)
		{
			if (key[i] != "x")
			{
				if (i < 27)
				{
					writer << (char)(i + 65) << " " << key[i] << " ";
				}
				else if (i >= 27)
				{
					writer << (char)(i + 70) << " " << key[i] << " ";
				}
			}
		}
		writer << "\n";
		for (int i = 0; i < text.size(); i++)
		{
			if ((text[i] == ' ' || text[i] == '.' || text[i] == '/' || text[i] == '?' || text[i] == '-' || text[i] == ',' || text[i] == '\'' || text[i] == '\"' || text[i] == '$' || text[i] == '!'))
			{
				writer << text[i];
			}
			else if ((int)text[i] - 'A'< 27)
			{
				writer << key[(int)text[i] - 'A'];
			}

			else if ((int)text[i] - 'A' >= 27)
			{
				writer << key[(int)text[i] + 27 - 'a'];
			}
		}
		return true;
	}
	bool setText()
	{
		ifstream reader("text.txt");
		std::string code1((istreambuf_iterator<char>(reader)), istreambuf_iterator<char>());
		for (int i = 0; i < code1.size(); i++)
			text.push_back(code1[i]);
		return true;
	}
	bool setKey()
	{
		char ctemp;
		string stemp;
		ifstream reader("key.txt");
		if (reader.is_open())
		{
			while (true)
			{
				if (reader.eof())
					break;
				reader >> ctemp;
				reader >> stemp;
				int index = ctemp - 'A';
				if (index <= 27)
				{
					key[index] = stemp;
				}
				else
				{
					key[ctemp + 27 - 'a'] = stemp;
				}
			}
			return true;
		}
		else
		{
			cout << "Error. Key File not found.";
			return false;
		}
		reader.close();
	}
	string decode()
	{
		string returnstring;
		static int k = 0, otherchar;
		int j = 0;
		while ((k < code.size()) && (j < code.size()))
		{
			for (int i = 0; (i < key.capacity()) && (j < code.size()); i++)
			{
				string decodestring;
				j = k;
				for (j; j < (key[i].length() + k) && (j < code.size()); j++)
				{

					if ((code[j] == ' ' || code[j] == ',' || code[j] == '.' || code[j] == '/' || code[j] == '!' || code[j] == '?' || code[j] == '_' || code[j] == '\'' || code[j] == '\"' || code[j] == '$') && (j == k))
					{
						returnstring += code[j];
						k += 1;
						j = k;
						continue;
					}
					decodestring += code[j];

				}
				if (key[i] == "x")
				{
					continue;
				}

				if (decodestring == key[i])
				{

					int adder = i;
					if (adder  >  26)
					{
						adder += 70;
					}
					else
					{
						adder += 65;
					}
					returnstring += (char)adder;
					k = j;
					otherchar = j;
					continue;
				}

				if (k < code.size())
				{
					j = k;
				}

			}
		}
		return returnstring;
	}
	void Setcode()
	{
		ifstream reader("code.txt");
		std::string code1((istreambuf_iterator<char>(reader)), istreambuf_iterator<char>());
		if (reader.is_open())
		{
			for (int i = 0; i < code1.length(); i++)
				code.push_back(code1[i]);
		}
		else
		{
			cout << "Error.Cannot open code file.";
		}
		reader.close();
	}
};

int main()
{
	GGG code1;
	char choice;
	cout << "Do you want to decode or encode? (d / e): ";
	cin >> choice;
	switch (choice)
	{
	case 'd':
		cout << "Paste the code in code.txt and key in key.txt and then ";
		system("pause");
		code1.setKey();
		code1.Setcode();
		cout << code1.decode();
		cout << "\n\n";
		system("pause");
		break;
	case 'e':
		cout << "Paste the code in code.txt and key in key.txt and then ";
		system("pause");
		code1.setKey();
		code1.setText();
		code1.encode();
		cout << "Get the coded text in encoded.txt file !";
		cout << "\n\n";
		system("pause");
		break;

	}
}