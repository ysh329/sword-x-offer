# string

## split

```cpp
#include <vector>
#include <string>

std::vector<std::string> split(const  std::string& s, const std::string& delim) {
    std::vector<std::string> res;
    size_t pos = 0;
    size_t len = s.length();
    size_t delim_len = delim.length();
    if(delim_len == 0) return res;
    while(pos < len) {
        int find_pos = s.find(delim, pos);
        if(find_pos < 0) {
            res.emplace_back(s.substr(pos, len - pos));
            break;
        }
        res.emplace_back(s.substr(pos, find_pos - pos));
        pos = find_pos + delim_len;
    }
    return res;
}
```
