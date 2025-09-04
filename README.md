using System;
using System.Collections.Generic;

public class OldPhonePad
{
    // Keypad mapping
    private static Dictionary<char, string> keypad = new Dictionary<char, string>()
    {
        {'2', "ABC"},
        {'3', "DEF"},
        {'4', "GHI"},
        {'5', "JKL"},
        {'6', "MNO"},
        {'7', "PQRS"},
        {'8', "TUV"},
        {'9', "WXYZ"},
        {'0', " "} // space
    };

    public static string Translate(string input)
    {
        string result = "";
        char lastKey = '\0';
        int pressCount = 0;

        foreach (char c in input)
        {
            if (c == '#')
            {
                // SEND → finalize message
                if (lastKey != '\0')
                {
                    result += GetLetter(lastKey, pressCount);
                }
                break;
            }
            else if (c == ' ')
            {
                // Pause → finalize current character
                if (lastKey != '\0')
                {
                    result += GetLetter(lastKey, pressCount);
                    lastKey = '\0';
                    pressCount = 0;
                }
            }
            else
            {
                if (c == lastKey)
                {
                    pressCount++;
                }
                else
                {
                    if (lastKey != '\0')
                    {
                        result += GetLetter(lastKey, pressCount);
                    }
                    lastKey = c;
                    pressCount = 1;
                }
            }
        }

        return result;
    }

    private static char GetLetter(char key, int count)
    {
        if (keypad.ContainsKey(key))
        {
            string letters = keypad[key];
            int index = (count - 1) % letters.Length;
            return letters[index];
        }
        return key; // fallback
    }
}
