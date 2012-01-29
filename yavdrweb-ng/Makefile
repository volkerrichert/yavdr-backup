YAVDRWEBDIR=../../yavdr-webfrontend/yavdrweb-ng/

all: backup.so

test: all testdeploy
	make -C ${YAVDRWEBDIR} testdeploy
	make -C ${YAVDRWEBDIR} test

testdeploy: all
	@mkdir -p /tmp/yavdrweb
	@cp -f sundtek.so /tmp/yavdrweb/
	@cp -pfr static /tmp/yavdrweb
	@cp -pfr ../templates /tmp/yavdrweb
	
backup.so: get_dvb.o set_dvb.o rescan.o remove_dvb.o
	${CXX} -o $@ $^ ${LDFLAGS}

.SUFFIXES: .ecpp .gif .jpg .css .js .cpp .png
EXTRA_ENV+="PATH=$$PATH:/usr/bin"
ECPPC=env ${EXTRA_ENV} ecppc -v
TNTNET=env ${EXTRA_ENV} tntnet
CXXFLAGS+=-I/usr/include -I/usr/include/yavdr -I/usr/include/ClearSilver -fPIC -g #-O2
#LDFLAGS+=-shared -L/usr/lib -ltntnet -lneo_utl -lneo_cs -lpam -lXNVCtrl -lX11 -lXext -ljson -lboost_regex -L/usr/lib/yavdr -lyavdrdb -lsvdrp -lyavdrutils

LDFLAGS+=-shared -L/usr/lib -ltntnet -lneo_utl -ljsoncpp -L/usr/lib/yavdr -lyavdrdb

.ecpp.cpp:
	${ECPPC} ${ECPPFLAGS} ${ECPPFLAGS_CPP} $<
.gif.cpp:
	${ECPPC} ${ECPPFLAGS} ${ECPPFLAGS_GIF} -b $<
.jpg.cpp:
	${ECPPC} ${ECPPFLAGS} ${ECPPFLAGS_JPG} -b $<
.png.cpp:
	${ECPPC} ${ECPPFLAGS} ${ECPPFLAGS_PNG} -b $<
.css.cpp:
	${ECPPC} ${ECPPFLAGS} ${ECPPFLAGS_CSS} -b $<
.js.cpp:
	${ECPPC} ${ECPPFLAGS} ${ECPPFLAGS_JS} -b $<
.cpp.o:
	${CXX} ${CPPFLAGS} ${CXXFLAGS} -c $<

clean:
	@rm -f *.o *.so *.cpp
	@rm -rf /tmp/yavdrweb
	
install:
	@mkdir -p $(DESTDIR)/usr/lib/yavdrweb
	@install backup.so $(DESTDIR)/usr/lib/yavdrweb/
	@cp -pr static $(DESTDIR)/usr/lib/yavdrweb

