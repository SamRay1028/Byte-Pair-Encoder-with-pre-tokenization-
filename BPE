#include <iostream>
#include <string_view>
#include <string.h>
#include <forward_list>
#include <string>
#include <chrono>
#include <unordered_map>
#include <fstream>
#include <bit>
#include <codecvt>
#include <unicode/utypes.h>
#include <unicode/ustring.h>
#include <vector>
#include <list>
#include <mach/mach.h>
#include <memory>
#include <any>
#include <variant>
#include <string_view>
#include "absl/container/flat_hash_map.h"
#include "absl/container/flat_hash_set.h"
#include "flat_hash_map.hpp"
#include "robin_hood.h"
#include "fibonacci.hpp"
#include "ordered_set.h"
#include "ordered_map.h"

size_t getMemoryUsage() {
    task_basic_info_data_t taskInfo;
    mach_msg_type_number_t infoCount = TASK_BASIC_INFO_COUNT;
    if (task_info(mach_task_self(), TASK_BASIC_INFO, (task_info_t)&taskInfo, &infoCount) == KERN_SUCCESS) {
        return taskInfo.resident_size; // RAM usage in bytes
    }
    return 0;
}

void task5(){
    int iterations = 32000;
    std::ifstream corpusText("/Users/samraya/Desktop/The complete works of William Shakespeare.txt");
    
    robin_hood::unordered_map<std::string, token> token_data;
    std::vector<std::string> words;
    robin_hood::unordered_map<std::string, int> word_frequencies;
    robin_hood::unordered_set<std::string> token_record;
    robin_hood::unordered_map<std::string, int> newFrequencies;
    
    std::vector<std::string> token_backing; // Keep backing storage alive
    //token_backing.reserve(100000);
    
    std::priority_queue<std::pair<int, std::string>> pq;
    //FibonacciHeap<std::pair<int, std::string>> pq;
    std::string line;
    std::string str;
    int pos1 = 0;
    int pos2 = 0;
    while(getline(corpusText, line)){
        line.insert(0, 1, '@');
        line.push_back('$');
        for(int i = 0; i < line.length() - 1; ++i){
            if(token_data[line.substr(i, 2)].frequency == 0){
                token_data[line.substr(i, 2)].split_index = 1;
            }
            ++token_data[line.substr(i, 2)].frequency;
        }
        for(int i = 0; i < line.length(); ++i){
            token_record.insert(std::string(1, line[i]));
            if(line[i] == ' ' || line[i] == '$'){
                // || line[i] == ',' || line[i] == '.' || line[i] == '-' || line[i] == ':' || line[i] == ';' || line[i] == '?' || line[i] == '/'
                ++pos2;
                str = line.substr(pos1, pos2 - pos1);
                if(word_frequencies[str] == 0){
                    //words[ID] = str;
                    words.push_back(str);
                    for(int j = pos1; j < pos2 - 1; ++j){
                        const auto& location = token_data[line.substr(j, 2)].words;
                        //location.size() == 0 || location[location.size() - 1] != ID
                        if(location.size() == 0 || location[location.size() - 1] != words.size() - 1){
                            //token_data[{line[j], line[j + 1]}].words.push_back(ID);
                            token_data[line.substr(j, 2)].words.push_back((int)words.size() - 1);
                        }
                    }
                    //++ID;
                }
                ++word_frequencies[str];
                --pos2;
                pos1 = pos2;
            }
            ++pos2;
        }
        pos1 = 0;
        pos2 = 0;
    }
    for(auto t : token_data){
        //pq.insert({t.second.frequency, t.first});
        pq.push({t.second.frequency, std::string(t.first)});
    }
    
    std::cout << "token_data size: " << token_data.size() << std::endl;
    std::cout << "word_frequencies size: " << word_frequencies.size() << std::endl;
    std::cout << "token_record size: " << token_record.size() << std::endl;
    
    //pq.removeMaximum();
    
    //pq.pop();
    
    
    int a = 0;
    int c = 0;
    int si = 0;
    std::string temp;
    std::string result;
    std::string top;
    std::string ID_A;
    for(int i = 0; i < iterations; ++i){
        //std::cout << "i: " << i <<", greatest: " << pq.top().second << ", words sizes: " << token_data[pq.top().second].words.size() << std::endl;
        //std::cout << "i: " << i <<", greatest: " << pq.top().second << std::endl;
        //token_data[pq.top().second].words.size()
        top = pq.top().second;
        si = token_data[top].split_index;
        for(int j = 0; j < token_data[top].words.size(); ++j){
            ID_A = words[token_data[top].words[j]];
            a = 0;
            c = 0;
            if(top.length() < ID_A.length()){
                if(top == ID_A.substr(0, top.length())){
                    a = 2;
                    while(top.length() + a < ID_A.length() && token_record.contains(ID_A.substr(top.length(), a))){
                        //token_record[ID_A.substr(top.length(), a)]
                        ++a;
                    }
                    --a;
                    temp = ID_A.substr(si, top.length() - si + a);
                    result = top.substr(0, si) + temp;
                    token_data[temp].frequency -= word_frequencies[ID_A];
                    token_data[result].frequency += word_frequencies[ID_A];
                    newFrequencies[temp] = token_data[temp].frequency;
                    newFrequencies[result] = token_data[result].frequency;
                    if(result.length() <= ID_A.length()){
                        token_data[result].words.push_back(token_data[top].words[j]);
                    }
                    auto b = std::find(token_data[temp].words.begin(), token_data[temp].words.end(), token_data[top].words[j]);
                    if(b != token_data[temp].words.end()){
                        token_data[temp].words.erase(b);
                    }
                    
                    token_data[result].split_index = top.length() - si;
                }
                for(int k = a + 1; k < ID_A.length() - top.length(); ++k){
                    if(top == ID_A.substr(k, top.length())){
                        a = 2;
                        while(top.length() + a < ID_A.length() && token_record.contains(ID_A.substr(top.length(), a))){
                            ++a;
                        }
                        --a;
                        temp = ID_A.substr(k + si, top.length() - si + a);
                        result = top.substr(0, si) + temp;
                        token_data[temp].frequency -= word_frequencies[ID_A];
                        token_data[result].frequency += word_frequencies[ID_A];
                        newFrequencies[temp] = token_data[temp].frequency;
                        newFrequencies[result] = token_data[result].frequency;
                        if(result.length() <= ID_A.length()){
                            token_data[result].words.push_back(token_data[top].words[j]);
                        }
                        auto b = std::find(token_data[temp].words.begin(), token_data[temp].words.end(), token_data[top].words[j]);
                        if(b != token_data[temp].words.end()){
                            token_data[temp].words.erase(b);
                        }
                        token_data[result].split_index = top.length();
                        //a = 2;
                        c = k - 2;
                        while(c > -1 && token_record.contains(ID_A.substr(c, k - c))){
                            --c;
                        }
                        ++c;
                        temp = ID_A.substr(c, k + si - c);
                        result = temp + top.substr(si, top.length() - si);
                        token_data[temp].frequency -= word_frequencies[ID_A];
                        token_data[result].frequency += word_frequencies[ID_A];
                        newFrequencies[temp] = token_data[temp].frequency;
                        newFrequencies[result] = token_data[result].frequency;
                        if(result.length() <= ID_A.length()){
                            token_data[result].words.push_back(token_data[top].words[j]);
                        }
                        b = std::find(token_data[temp].words.begin(), token_data[temp].words.end(), token_data[top].words[j]);
                        if(b != token_data[temp].words.end()){
                            token_data[temp].words.erase(b);
                        }
                        token_data[result].split_index = (int) result.length() - top.length();
                        
                        //
                        --k;
                        k += top.length() + a;
                        //
                    }
                }
                if(top == ID_A.substr(ID_A.length() - top.length(), top.length())){
                    c = (int) (ID_A.length() - top.length()) - 1;
                    while(c > -1 && token_record.contains(ID_A.substr(c, ID_A.length() - top.length() - c))){
                        --c;
                    }
                    ++c;
                    temp = ID_A.substr(c, ID_A.length() - top.length() - c +  si);
                    result = temp + top.substr(si, top.length() -  si);
                    token_data[temp].frequency -= word_frequencies[ID_A];
                    token_data[result].frequency += word_frequencies[ID_A];
                    newFrequencies[temp] = token_data[temp].frequency;
                    newFrequencies[result] = token_data[result].frequency;
                    if(result.length() <= ID_A.length()){
                        token_data[result].words.push_back(token_data[top].words[j]);
                    }
                    auto b = std::find(token_data[temp].words.begin(), token_data[temp].words.end(), token_data[top].words[j]);
                    if(b != token_data[temp].words.end()){
                        token_data[temp].words.erase(b);
                    }
                    token_data[result].split_index = (int) temp.length() - si;
                }
            }
            else if(top.length() == ID_A.length()){
                
            }
        }
        if(i == 31999){
            std::cout << pq.top().second << " frequency: " << token_data[pq.top().second].frequency << std::endl;
            for(int i = 0; i < token_data[pq.top().second].words.size(); ++i){
                std::cout << "word: " << words[token_data[pq.top().second].words[i]] << std::endl;
            }
        }
        
        //token_record[top] = true;
        token_record.insert(top);
        token_data.erase(top);
        
        //pq.removeMaximum();
        pq.pop();
        for(auto pair : newFrequencies){
            if(token_data[pair.first].frequency != 0){
                //pq.insert({pair.second, pair.first});
                pq.push({pair.second, pair.first});
            }
            
        }
        newFrequencies.clear();
        while(pq.top().first != token_data[pq.top().second].frequency){
            //pq.removeMaximum();
            pq.pop();
        }
    }
    
    
    
    
    /*
     robin_hood::unordered_map<std::string_view, token> token_data;
     std::vector<std::string> words;
     robin_hood::unordered_map<std::string, int> word_frequencies;
     robin_hood::unordered_set<std::string> token_record;
     robin_hood::unordered_map<std::string, int> newFrequencies;
     */
    /*
    std::cout << std::endl;
    std::cout << "token_data size: " << token_data.size() << std::endl;
    std::cout << "word_frequencies size: " << word_frequencies.size() << std::endl;
    std::cout << "token_record size: " << token_record.size() << std::endl;
     */
    
    std::cout << std::endl;
    double total = 0.0;
    double count = 0.0;
    for(auto it = token_data.begin(); it != token_data.end(); ++it){
        total += it->second.words.size();
        count++;
    }
    std::cout << "average words size: " << total / count << std::endl;
    std::cout << "frequency of e_: " << token_data[","].frequency << std::endl;
}

int main(int argc, const char * argv[]) {
    //absl::flat_hash_map<std::string, int> word_counts;
    //word_counts["apple"] = 5;
    //std::cout << word_counts["apple"] << std::endl;
    
    std::cout << "hello" << std::endl;
    auto start = std::chrono::high_resolution_clock::now();
    //size_t ramUsage1 = getMemoryUsage();
    printMemoryUsage();
    task5();
    printMemoryUsage();
    //size_t ramUsage2 = getMemoryUsage();
    auto end = std::chrono::high_resolution_clock::now();
    //std::cout << "RAM Usage: " << ramUsage2 - ramUsage1 << " bytes" << std::endl;
    std::cout << "Time: " << std::chrono::duration<double>(end - start).count() << " s" << std::endl;
    
    /*
    std::string s1 = "Hello";
    std::string s2 = " World";
    std::string_view sv = s1 + s2;
    std::cout << "sv: " << sv << std::endl;
     */
    /*
    FibonacciHeap<int> fh;
    fh.insert(-1);
    fh.insert(-10);
    fh.insert(-3);
     */
    //std::cout << fh.getMaximum() << std::endl;
    return 0;
}
