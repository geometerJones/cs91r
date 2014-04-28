#
# Makefile for Open Pure CFR
# Richard Gibson, June 26, 2013
#

#OPT = -Wall -O3 -ffast-math -funroll-all-loops -ftree-vectorize -DHAVE_MMAP
OPT = -O0 -Wall -g -fno-inline

PURE_CFR_FILES = pure_cfr.o acpc_server_code/game.o acpc_server_code/rng.o constants.o parameters.o utility.o card_abstraction.o action_abstraction.o betting_node.o entries.o abstract_game.o player_module.o pure_cfr_machine.o

PRINT_PLAYER_STRATEGY_FILES = print_player_strategy.o player_module.o acpc_server_code/game.o acpc_server_code/rng.o constants.o parameters.o utility.o card_abstraction.o action_abstraction.o betting_node.o entries.o abstract_game.o

PURE_CFR_PLAYER_FILES = pure_cfr_player.o player_module.o acpc_server_code/game.o acpc_server_code/rng.o acpc_server_code/net.o constants.o parameters.o utility.o card_abstraction.o action_abstraction.o betting_node.o entries.o abstract_game.o

CFLAGS     ?=-std=c99 -Wall -g -O2
LIBS = -lm

SRC        :=$(addprefix src/,deck.c hand_index.c)
OBJ        :=$(SRC:.c=.o)

TARGETS    :=src/check
TARGET_SRC :=$(addprefix src/,$(TARGETS:=-main.c))
TARGET_OBJ :=$(TARGET_SRC:.c=.o)

.PHONY: all clean check 

#clean:
#	rm -f $(TARGETS)
#	rm -f $(MAIN_OBJ) src/check-main.o $(OBJ)

#check: src/check
#	./src/check

src/check: src/check-main.o $(OBJ)

$(TARGETS):
	$(CC) $(LDFLAGS) -o $@ $^ $(LOADLIBES) $(LDLIBS) $(LIBS)

src/check-main.o src/hand_index.o: src/deck.h src/hand_index.h
src/deck.o: src/deck.h 

all: $(TARGETS) pure_cfr print_player_strategy pure_cfr_player

%.o: %.cpp
	$(CXX) $(OPT) -c $^

pure_cfr: $(PURE_CFR_FILES)
	$(CXX) $(OPT) -pthread -o $@ $(PURE_CFR_FILES) src/deck.o src/hand_index.o

print_player_strategy: $(PRINT_PLAYER_STRATEGY_FILES)
	$(CXX) $(OPT) -o $@ $(PRINT_PLAYER_STRATEGY_FILES) src/deck.o src/hand_index.o

pure_cfr_player: $(PURE_CFR_PLAYER_FILES)
	$(CXX) $(OPT) -o $@ $(PURE_CFR_PLAYER_FILES) src/deck.o src/hand_index.o

#linkingstuff: 
#	$(CXX) -o pure_cfr $(PURE_CFR_FILES) src/deck.o src/hand_index.o

clean: 
	-rm *.o acpc_server_code/*.o
	-rm pure_cfr print_player_strategy pure_cfr_player
	rm -f $(TARGETS)
	rm -f $(MAIN_OBJ) src/check-main.o $(OBJ)