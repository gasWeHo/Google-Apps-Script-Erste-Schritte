# Google1
Erste Schritte
Wäre des ohne den Maestro möglich?

## Unser erstes GAS-Script

Ich bin ein Link
["C++ `std::variant` versus `std::any`"](https://stackoverflow.com/questions/56303939/c-stdvariant-vs-stdany/)

Wir behandeln hier 3 Themen:

  * Thema 1:
  * Thema 2:
  * Thema 3:
  * Thema 4:
  * Thema 5:


Unser erstes Beispiel:

```js
function onLoad(){
  let ui = SpreadsheetApp.getUi();
  // Or DocumentApp or FormApp.
  ui.createMenu('Haushalt')
      .addItem('Monat kopieren', 'triggerMonatKopieren')
      .addItem('Datei sichern', 'triggerDateiSichern')
      .addItem('Monat löschen', 'triggerMonatLöschen')
      .addItem('Jahr löschen', 'triggerJahrLöschen')
      .addToUi();
}
```

[Was ist ein Maestromuster?](Maestro.md)

[Was ist ein Wernermuster?](Werner.md)

<img src="Google_Apps_Script.svg.png" width="40">

Mein erstes Listing mit Zeilennummern:

```js
01: function onLoad(){
02:   let ui = SpreadsheetApp.getUi();
03:   // Or DocumentApp or FormApp.
04:   ui.createMenu('Haushalt')
05:       .addItem('Monat kopieren', 'triggerMonatKopieren')
06:       .addItem('Datei sichern', 'triggerDateiSichern')
07:       .addItem('Monat löschen', 'triggerMonatLöschen')
08:       .addItem('Jahr löschen', 'triggerJahrLöschen')
09:       .addToUi();                                       
10: }
```

Ein zweites Beispiel zu Code

<pre>
01: function onLoad(){
02:   let ui = SpreadsheetApp.getUi();
03:   // Or DocumentApp or FormApp.
04:   <b>ui.createMenu('Haushalt')</b>
05:       .addItem('Monat kopieren', 'triggerMonatKopieren')
06:       .addItem('Datei sichern', 'triggerDateiSichern')
07:       .addItem('Monat löschen', 'triggerMonatLöschen')
08:       .addItem('Jahr löschen', 'triggerJahrLöschen')
09:       .addToUi();                                       
10: }
</pre>



Und hier kommt ein zweites Beispiel: kleine Änderung !!!

```cpp
001: // =====================================================================================
002: // Program.cpp
003: // =====================================================================================
004: // hier haben wir cpp-Code kleine Änder
005: #include <iostream>
006: #include <string>
007: #include <vector>
008: #include <exception>
009: #include <ostream>
010: #include <stdexcept>
011: #include <iomanip>
012: #include <sstream>
013: 
014: #include <windows.h>
015: #include <numeric>
016: 
017: // https://stackoverflow.com/questions/14762456/getclipboarddatacf-text
018: 
019: namespace Clipboard {
020: 
021:     const std::string_view Version =
022:         std::string_view(
023:             "Code-Converter of C++ Code Snippets - Version 1.01 (31.05.2021)\n"
024:             "Copyright (C) 2021 Peter Loos");
025: 
026:     // non RAII conformant solution
027:     int transformText_Deprecated() {
028: 
029:         // open the clipboard, and empty it.
030:         if (!OpenClipboard((HWND)0))
031:             return -1;
032: 
033:         // get handle of clipboard object for ANSI text
034:         HANDLE data = GetClipboardData(CF_TEXT);
035:         if (data == nullptr)
036:             return -1;
037: 
038:         // lock the handle to get the actual text pointer
039:         char* m_text = static_cast<char*>(GlobalLock(data));
040:         if (m_text == nullptr)
041:             return -1;
042: 
043:         // save text in a string class instance
044:         std::string text{ m_text };
045:         std::cout << "TEXT: " << text << std::endl;
046: 
047:         // release lock
048:         GlobalUnlock(data);
049: 
050:         // release clipboard
051:         CloseClipboard();
052: 
053:         return 1;
054:     }
055: 
056:     // RAII conformant solution
057:     class RAIIClipboard
058:     {
059:     public:
060:         RAIIClipboard() {
061:             if (!OpenClipboard((HWND)0))
062:                 throw std::runtime_error("Can't open clipboard.");
063:         }
064: 
065:         ~RAIIClipboard() {
066:             CloseClipboard();
067:         }
068: 
069:         // prevent copy semantics
070:         RAIIClipboard(const RAIIClipboard&) = delete;
071:         RAIIClipboard& operator=(const RAIIClipboard&) = delete;
072:     };
073: 
074:     class RAIIClipboardReader
075:     {
076:     public:
077:         explicit RAIIClipboardReader()
078:         {
079:             RAIIClipboard clipboard;
080: 
081:             m_data = GetClipboardData(CF_TEXT);
082:             if (m_data == nullptr)
083:                 throw std::runtime_error("ERROR GetClipboardData!");
084: 
085:             m_text = static_cast<const char*>(GlobalLock(m_data));
086:             if (m_text == nullptr) {
087:                 // take care of formerly allocated resources (!)
088:                 GlobalFree(m_data);
089:                 throw std::runtime_error("ERROR GlobalLock!");
090:             }
091:         }
092: 
093:         // prevent copy semantics
094:         RAIIClipboardReader(const RAIIClipboardReader&) = delete;
095:         RAIIClipboardReader& operator=(const RAIIClipboardReader&) = delete;
096: 
097:         std::string getText() const {
098:             return std::string{ m_text };
099:         }
100: 
101:         ~RAIIClipboardReader() {
102:             if (m_data != nullptr)
103:                 GlobalUnlock(m_data);
104:         }
105: 
106:     private:
107:         HANDLE m_data;
108:         const char* m_text;
109:     };
110: 
111:     class RAIIClipboardWriter
112:     {
113:     public:
114:         explicit RAIIClipboardWriter(const std::string& text) {
115: 
116:             // allocate a buffer for the text
117:             size_t len = text.size();
118: 
119:             m_data = GlobalAlloc(GMEM_MOVEABLE, len + 1);
120:             if (m_data == nullptr)
121:                 throw std::runtime_error("ERROR GlobalAlloc!");
122: 
123:             m_text = static_cast<char*>(GlobalLock(m_data));
124:             if (m_text == nullptr) {
125:                 // take care of formerly allocated resources (!)
126:                 GlobalFree(m_data);
127:                 throw std::runtime_error("ERROR GlobalLock!");
128:             }
129: 
130:             // copy the text
131:             memcpy(m_text, text.c_str(), len);
132:             m_text[len] = 0;
133: 
134:             RAIIClipboard clipboard;
135:             EmptyClipboard();
136:             SetClipboardData(CF_TEXT, m_data);
137:         }
138: 
139:         // prevent copy semantics
140:         RAIIClipboardWriter(const RAIIClipboardWriter&) = delete;
141:         RAIIClipboardWriter& operator=(const RAIIClipboardWriter&) = delete;
142: 
143:         ~RAIIClipboardWriter() {
144:             if (m_text != nullptr)
145:                 GlobalUnlock(m_data);
146:             if (m_data != nullptr)
147:                 GlobalFree(m_data);
148:         }
149: 
150:     private:
151:         HGLOBAL m_data{ nullptr };
152:         char* m_text{ nullptr };
153:     };
154: 
155:     class CodeConverter
156:     {
157:     private:
158:         std::vector<std::string> m_code;
159:         std::string m_text;
160:         std::string m_conv;
161: 
162:     public:
163:         CodeConverter() = delete;
164:         explicit CodeConverter(std::string text) : m_text(text) {};
165: 
166:         // public interface
167:         void setText(const std::string& text) { m_text = text; }
168: 
169:         std::string getText() { return m_conv; }
170: 
171:         void parseInput() {
172:             try
173:             {
174:                 if (m_text.empty()) {
175:                     std::cout << "Empty Clipboard !!!" << std::endl;
176:                     return;
177:                 }
178: 
179:                 std::string pattern{ "\r\n" };
180:                 std::string::iterator it = m_text.begin();
181: 
182:                 size_t first{};
183:                 size_t len{};
184:                 size_t index{};
185: 
186:                 while ((index = m_text.find_first_of(pattern, first)) != std::string::npos) {
187: 
188:                     len = index - first;
189:                     std::string line = m_text.substr(first, len);
190:                     m_code.push_back(line);
191:                     first = index + 2; // skipping '\r' and '\n'
192:                     // std::cout << "Found: " << line << std::endl;
193:                 }
194: 
195:                 // retrieve last line
196:                 std::string line = m_text.substr(first);
197:                 // std::cout << "Last:  " << line << std::endl;
198: 
199:                 if (!line.empty()) {
200:                     m_code.push_back(line);
201:                 }
202:             }
203:             catch (const std::exception& e)
204:             {
205:                 std::cerr << "ERROR: " << e.what() << std::endl;
206:             }
207:         }
208: 
209:         void addLineNumbers() {
210: 
211:             // assuming no code blocks longer than 1000 lines :-)
212:             int indent = (m_code.size() < 100) ? 2 : 3;
213: 
214:             m_conv = std::accumulate(
215:                 std::begin(m_code),
216:                 std::end(m_code),
217:                 std::string(""),
218:                 [indent, counter = 0](const std::string& first, const std::string& next) mutable {
219: 
220:                 counter++;
221:                 std::ostringstream ss;
222:                 ss << std::setfill('0') << std::setw(indent)
223:                     << counter << ": " << next << std::endl;
224: 
225:                 return first + ss.str();
226:             });
227:         }
228: 
229:         void printStats() {
230:             std::cout << Version << std::endl;
231:             std::cout << "Converted " << m_code.size() << " lines!" << std::endl;
232:             std::cout << "Press <enter> to quit ... " << std::endl;
233:         }
234:     };
235: 
236:     void transformText() {
237:         try
238:         {
239:             RAIIClipboardReader reader;
240:             std::string code = reader.getText();
241:             CodeConverter conv{ code };
242:             conv.parseInput();
243:             conv.addLineNumbers();
244:             conv.printStats();
245:             std::string convertedText = conv.getText();
246:             RAIIClipboardWriter writer{ convertedText };
247: 
248:             char ch;
249:             std::cin >> ch;
250:         }
251:         catch (const std::exception& e)
252:         {
253:             std::cerr << "ERROR: " << e.what() << std::endl;
254:         }
255:     }
256: }
257: 
258: int main()
259: {
260:     using namespace Clipboard;
261:     transformText();
262: }
263: 
264: // =====================================================================================
265: // End-of-File
266: // =====================================================================================

```