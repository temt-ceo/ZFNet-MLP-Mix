## Ordered Data Structures in C++
### Linear Structures
#### Arrays
・ Elements are all the same type.<br>
・ The size of the type od data is known.<br>
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



