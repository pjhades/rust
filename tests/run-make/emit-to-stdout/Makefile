include ../../run-make-fulldeps/tools.mk

SRC=test.rs
OUT=$(TMPDIR)/out

all: asm llvm-ir dep-info mir llvm-bc obj metadata link multiple-types multiple-types-option-o

asm: $(OUT)
	$(RUSTC) --emit asm=$(OUT)/$@ $(SRC)
	$(RUSTC) --emit asm=- $(SRC) | diff - $(OUT)/$@
llvm-ir: $(OUT)
	$(RUSTC) --emit llvm-ir=$(OUT)/$@ $(SRC)
	$(RUSTC) --emit llvm-ir=- $(SRC) | diff - $(OUT)/$@
dep-info: $(OUT)
	$(RUSTC) --emit dep-info=$(OUT)/$@ $(SRC)
	sed -i '.back' '1s/.*:\(.*\)/-:\1/g' $(OUT)/$@
	$(RUSTC) --emit dep-info=- $(SRC) | diff - $(OUT)/$@
mir: $(OUT)
	$(RUSTC) --emit mir=$(OUT)/$@ $(SRC)
	$(RUSTC) --emit mir=- $(SRC) | diff - $(OUT)/$@

llvm-bc: $(OUT)
	$(RUSTC) --emit llvm-bc=- $(SRC) 1>/dev/tty 2>$(OUT)/$@ || true
	diff $(OUT)/$@ emit-llvm-bc.stderr
obj: $(OUT)
	$(RUSTC) --emit obj=- $(SRC) 1>/dev/tty 2>$(OUT)/$@ || true
	diff $(OUT)/$@ emit-obj.stderr
metadata: $(OUT)
	$(RUSTC) --emit metadata=- $(SRC) 1>/dev/tty 2>$(OUT)/$@ || true
	diff $(OUT)/$@ emit-metadata.stderr
link: $(OUT)
	$(RUSTC) --emit link=- $(SRC) 1>/dev/tty 2>$(OUT)/$@ || true
	diff $(OUT)/$@ emit-link.stderr

multiple-types: $(OUT)
	$(RUSTC) --emit asm=- --emit llvm-ir=- --emit dep-info=- --emit mir=- $(SRC) 2>$(OUT)/$@ || true
	diff $(OUT)/$@ emit-multiple-types.stderr

multiple-types-option-o: $(OUT)
	$(RUSTC) -o - --emit asm,llvm-ir,dep-info,mir $(SRC) 2>$(OUT)/$@ || true
	diff $(OUT)/$@ emit-multiple-types.stderr

$(OUT):
	mkdir -p $(OUT)
