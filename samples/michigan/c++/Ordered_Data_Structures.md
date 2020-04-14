## Ordered Data Structures in C++
### Linear Structures
#### Arrays
・ Elements are all the same type.<br>
・ The size of the type of data is known.(指定のIndexまでのメモリ = Index ✖️ 固定のメモリ量)<br>
・ Arrays must store their data sequentially in memory<br>
　　(The capacity of an array is the maximum number of elements that can be stored.).<br>
　　(より大きなメモリを割り当て、そこに内容物を全てコピーすることで増やすことができる。)<br>
**main.cpp**<br>
```
#include <iostream>
#include <vector>

int main() {
  // Create a fixed sized array of 9 primes:
  int values[9] = { 2, 3, 5, 7, 11, 13, 17, 19, 23 };
  Cube cubes[3] = { Cube(11), Cube(42), Cube(400) };
  std::vector<int> values2{ 2, 3, 5, 7, 11, 13, 17, 19, 23 }
  
  // Outputs the 4th prime.
  std::cout << values[3] << std::endl;
  // Print the size of each type `int`:
  std::cout << sizeof(int) << std::endl; // -> 4
  std::cout << sizeof(Cube) << std::endl; // -> 8 (bytes)

  // standard vector = dinamically growing array libraly
  std::cout << "Capacity:" << values2.capacity() << std::endl; // -> 9
  values2.push_back(23));
  std::cout << "Size:" << values2.size() << std::endl; // -> 10
  std::cout << "Capacity:" << values2.capacity() << std::endl; // -> 18(vectorのサイズをそのまま増やしている)

  // the offset from the beginning of the array to [2]:
  int offset = (long)&(values[2]) - (long)&(values[0]) # &によりアドレスを取得
  std::cout << offset << "  " << (long)&(values[2]) << std::endl; // -> (2 x 4 = )8  140734619896488
  
  return 0;
}
```

**Makefile**<br>
```
EXE = main
OBJS = main.o
CLEAN_RM =

include ../../_make/generic.mk
  ↓↓↓
# Compiler/linker config and object/depfile directory:
CXX = g++
LD  = g++
OBJS_DIR = .objs

# -MMD and -MP asks clang++ to generate a .d file listing the headers used in the source code for use in the Make process.
DEPFILE_FLAGS = -MMD -MP　#   -MMD: "Write a depfile containing user headers"、 -MP : "Create phony target for each dependency (other than main file)" (https://clang.llvm.org/docs/ClangCommandLineReference.html)

# Flags for compile:
CXXFLAGS += -std=c++14 -O0 $(WARNINGS) $(DEPFILE_FLAGS) -g -c $(ASANFLAGS)

# Flags for linking:
LDFLAGS += -std=c++14 $(ASANFLAGS)

all: $(EXE) # Rule for `all` (first/default rule):

$(EXE): $(patsubst %.o, $(OBJS_DIR)/%.o, $(OBJS)) # Rule for linking the final executable - $(EXE) depends on all object files in $(OBJS)
	$(LD) $^ $(LDFLAGS) -o $@

$(OBJS_DIR): # Ensure .objs/ exists:
	@mkdir -p $(OBJS_DIR)
	@mkdir -p $(OBJS_DIR)/uiuc

$(OBJS_DIR)/%.o: %.cpp | $(OBJS_DIR) # Rules for compiling source code.
	$(CXX) $(CXXFLAGS) $< -o $@

-include $(OBJS_DIR)/*.d # Additional dependencies for object files 
-include $(OBJS_DIR)/uiuc/*.d

# Standard C++ Makefile rules:
clean:
	rm -rf $(EXE) $(TEST) $(OBJS_DIR) $(CLEAN_RM) *.o *.d

tidy: clean
	rm -rf doc

.PHONY: all tidy clean
}
```

#### Arrays
**main.cpp**<br>
```
#include <iostream>

int main() {
  // Create a fixed sized array of 9 primes:
  int values[9] = { 2, 3, 5, 7, 11, 13, 17, 19, 23 };
  
  // Outputs the 4th prime.
  std::cout << values[3] << std::endl;
  return 0;
}
```



