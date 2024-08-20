type = markLAPACK
#flto = -flto=auto
flto = 
COMPILER = gfortran

LINKER = gfortran

# For more compilation error warnings
#COPTIONS = -c -Wextra -Wall -Wunused -Wno-tabs -Wno-compare-reals -Wno-maybe-uninitialized -Wno-unused-dummy-argument -Wno-ampersand -Wno-conversion -D IEEE -fimplicit-none -fbounds-check -m64 -funroll-loops -ftree-vectorize -ffpe-summary='invalid','zero','overflow','underflow' -fno-unsafe-math-optimizations -frounding-math -fsignaling-nans -O3 -march=native

# figure out the OS, and tweak static
ifeq ($(OS),Windows_NT)
	detected_OS := Windows
 	static_flag= -static -static-libgfortran 
 	#static_flag= -static-libgfortran 
 	#static_flag=  
	file_exten=.exe
else
 	detected_OS := $(shell uname)  # same as "uname -s"
 	static_flag= -static-libgfortran
      file_exten=
endif

ifeq ($(type),mark32)
	# For compilation with dble precision and LinPack as 32-bit compilation
	COPTIONS = -std=f2018 -c -D ieee -D dbleprecision -fimplicit-none -fbounds-check -m32 -funroll-loops -ftree-vectorize -ffpe-summary='invalid','zero','overflow','underflow' -fno-unsafe-math-optimizations -frounding-math -fsignaling-nans -O3 $(flto)
	#COPTIONS = -std=f2018 -c -D ieee -D dbleprecision -m32 -fsignaling-nans -ffpe-summary='invalid','zero','overflow','underflow' -O3 -funroll-loops -ffast-math -march=native $(flto)
	LINKLIBS = Linpack32.a
	MFLAG = -m32
	EXECUTABLE = mark32$(file_exten)

else ifeq ($(type),mark64)
	# For compilation with dble precision and LinPack
	COPTIONS = -std=f2018 -c -D ieee -D dbleprecision -m64 -fsignaling-nans -ffpe-summary='invalid','zero','overflow','underflow' -O3 -funroll-loops -ffast-math $(flto)
	#COPTIONS = -std=f2018 -c  -D ieee -D dbleprecision -Wextra -Wall -Wunused -Wno-tabs -Wno-compare-reals -Wno-maybe-uninitialized -Wno-unused-dummy-argument -Wno-ampersand -Wno-conversion -fimplicit-none -fbounds-check -m64 -funroll-loops -ftree-vectorize -ffpe-summary='invalid','zero','overflow','underflow' -fno-unsafe-math-optimizations -frounding-math -fsignaling-nans -O3
  	MFLAG = -m64
  	LINKLIBS = Linpack.a
  	EXECUTABLE = mark64$(file_exten)

else ifeq ($(type),mark128)
	# For compilation with quad precision and LinPack
	COPTIONS = -std=f2018 -c -D ieee -D quadprecision -m64 -fsignaling-nans -ffpe-summary='invalid','zero','overflow','underflow' -O3 -funroll-loops -ffast-math -march=native $(flto)
	MFLAG = -m64
	LINKLIBS = Linpack.a
	EXECUTABLE = mark128$(file_exten)

else ifeq ($(type),marksngl)
	# For compilation with single precision and LinPack
	COPTIONS = -std=f2018 -c -D ieee -D snglprecision -fimplicit-none -fbounds-check -m64 -funroll-loops -ftree-vectorize -ffpe-summary='invalid','zero','overflow','underflow' -fno-unsafe-math-optimizations -frounding-math -fsignaling-nans -O3 -march=native $(flto) 
	MFLAG = -m64
	LINKLIBS = Linpack.a
	EXECUTABLE = marksngl$(file_exten)

else ifeq ($(type),markLAPACK)
	# For compilation with LAPACK
	COPTIONS = -std=f2018 -c -D ieee -D lapack -m64 -fsignaling-nans -ffpe-summary='invalid','zero','overflow','underflow' -O3 -funroll-loops -ffast-math -march=native $(flto)
  	MFLAG = -m64
  	LINKLIBS = -llapack -lblas
	#LINKLIBS = /usr/lib/x86_64-linux-gnu/liblapack.so /usr/lib/x86_64-linux-gnu/libblas.so
	# euler
	#LINKLIBS = /usr/lib64/liblapack.so /usr/lib64/libblas.so 
	# alpine 
	#LINKLIBS =  -L $(CURC_MKL_LIB) -Wl,--no-as-needed -lmkl_gf_lp64 -lmkl_sequential -lmkl_core -lpthread -lm -ldl
  	EXECUTABLE = markLAPACK$(file_exten)
endif

OBJECTS = mark.o glabrd.o xmatrx.o tmread.o rlabrd.o blabrd.o dlabrd.o \
estmat.o varmat.o derivedest.o piread.o func.o saturd.o \
chprob.o chprob001.o chprob002.o chprob008.o chprob009.o chprob032.o chprob115.o \
chprob119.o chprob121.o chprob126.o chprob139.o chprob140.o chprob141.o \
chprob142.o chprob143.o chprob144.o chprob160.o chprob170.o chprob171.o \
chprob172.o chprob173.o chprob174.o chprob175.o chprob176.o chprob177.o \
chprob178.o chprob179.o chprob180.o chprob181.o chprob182.o chprob183.o \
chprob184.o rcread.o kfread.o nsread.o  optmiz.o status_module.o \
prcisub.o prfunc.o mcmc.o hyperread.o gibbsitsub.o optimizers_module.o \
gaussquad.o hyper_dist_module.o profile_conf_interval_module.o data_module.o \
design_matrix_funcs_module.o random_values_module.o

$(EXECUTABLE): $(OBJECTS)
	$(LINKER) $(MFLAG) -fopenmp $(flto) $(OBJECTS) $(LINKLIBS) -o $(EXECUTABLE) $(static_flag) 

status_module.mod status_module.o: status_module.f90
	$(COMPILER) -fall-intrinsics -cpp $(COPTIONS) status_module.f90
hyper_dist_module.mod hyper_dist_module.o: hyper_dist_module.f90
	$(COMPILER) $(COPTIONS) hyper_dist_module.f90
random_values_module.mod random_values_module.o: random_values_module.f90 status_module.mod
	$(COMPILER) $(COPTIONS) -fall-intrinsics random_values_module.f90
profile_conf_interval_module.mod profile_conf_interval_module.o: profile_conf_interval_module.f90
	$(COMPILER) $(COPTIONS) profile_conf_interval_module.f90
data_module.mod data_module.o: data_module.f90 status_module.mod design_matrix_funcs_module.mod
	$(COMPILER) $(COPTIONS) data_module.f90
design_matrix_funcs_module.mod design_matrix_funcs_module.o: design_matrix_funcs_module.f90
	$(COMPILER) $(COPTIONS) design_matrix_funcs_module.f90
optimizers_module.mod optimizers_module.o: optimizers_module.f90 status_module.f90  random_values_module.f90
	$(COMPILER) -cpp $(COPTIONS) optimizers_module.f90
mark.o: mark.f90 status_module.mod data_module.mod
	$(COMPILER) -cpp $(COPTIONS) -fall-intrinsics -fopenmp mark.f90
gibbsitsub.o: gibbsitsub.f90 status_module.mod
	$(COMPILER) $(COPTIONS) gibbsitsub.f90
glabrd.o: glabrd.f90  status_module.mod
	$(COMPILER) $(COPTIONS) glabrd.f90
rlabrd.o: rlabrd.f90  status_module.mod
	$(COMPILER) $(COPTIONS) rlabrd.f90
blabrd.o: blabrd.f90  status_module.mod
	$(COMPILER) $(COPTIONS) blabrd.f90
dlabrd.o: dlabrd.f90  status_module.mod
	$(COMPILER) $(COPTIONS) dlabrd.f90
xmatrx.o: xmatrx.f90  status_module.mod data_module.mod
	$(COMPILER) -cpp $(COPTIONS) xmatrx.f90
estmat.o: estmat.f90  status_module.mod design_matrix_funcs_module.mod data_module.mod profile_conf_interval_module.mod hyper_dist_module.mod random_values_module.mod
	$(COMPILER) -cpp $(COPTIONS) estmat.f90
varmat.o: varmat.f90  status_module.mod data_module.mod
	$(COMPILER) -cpp $(COPTIONS) varmat.f90
derivedest.o: derivedest.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) derivedest.f90
piread.o: piread.f90  status_module.mod
	$(COMPILER) $(COPTIONS) piread.f90
kfread.o: kfread.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) kfread.f90
nsread.o: nsread.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) nsread.f90
rcread.o: rcread.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) rcread.f90
tmread.o: tmread.f90  status_module.mod
	$(COMPILER) $(COPTIONS) tmread.f90
hyperread.o: hyperread.f90  status_module.mod hyper_dist_module.mod
	$(COMPILER) $(COPTIONS) hyperread.f90
func.o: func.f90 status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) -fopenmp func.f90
chprob.o: chprob.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob.f90
chprob001.o: chprob001.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob001.f90
chprob002.o: chprob002.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob002.f90
chprob008.o: chprob008.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob008.f90
chprob009.o: chprob009.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob009.f90
chprob032.o: chprob032.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob032.f90
chprob115.o: chprob115.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob115.f90
chprob119.o: chprob119.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob119.f90
chprob121.o: chprob121.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob121.f90
chprob126.o: chprob126.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob126.f90
chprob139.o: chprob139.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob139.f90
chprob140.o: chprob140.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob140.f90
chprob141.o: chprob141.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob141.f90
chprob142.o: chprob142.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob142.f90
chprob143.o: chprob143.f90 status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob143.f90
chprob144.o: chprob144.f90 status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob144.f90
chprob160.o: chprob160.f90 status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob160.f90
chprob170.o: chprob170.f90 status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob170.f90
chprob171.o: chprob171.f90 status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob171.f90
chprob172.o: chprob172.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob172.f90
chprob173.o: chprob173.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob173.f90
chprob174.o: chprob174.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob174.f90
chprob175.o: chprob175.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob175.f90
chprob176.o: chprob176.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob176.f90
chprob177.o: chprob177.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob177.f90
chprob178.o: chprob178.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob178.f90
chprob179.o: chprob179.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob179.f90
chprob180.o: chprob180.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob180.f90
chprob181.o: chprob181.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob181.f90
chprob182.o: chprob182.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob182.f90
chprob183.o: chprob183.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob183.f90
chprob184.o: chprob184.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) chprob184.f90
chprob2.o: chprob2.f90  status_module.mod
	$(COMPILER) $(COPTIONS) chprob2.f90
matprt.o: matprt.f90 status_module.mod
	$(COMPILER) $(COPTIONS) matprt.f90
saturd.o: saturd.f90  status_module.mod data_module.mod
	$(COMPILER) $(COPTIONS) saturd.f90
optmiz.o: optmiz.f90  status_module.mod optimizers_module.mod
	$(COMPILER) $(COPTIONS) optmiz.f90
mcmc.o: mcmc.f90 status_module.mod hyper_dist_module.mod data_module.mod random_values_module.mod design_matrix_funcs_module.mod
	$(COMPILER) -fall-intrinsics -cpp $(COPTIONS) mcmc.f90
prcisub.o: prcisub.f90 status_module.mod optimizers_module.mod profile_conf_interval_module.mod
	$(COMPILER) $(COPTIONS) prcisub.f90
prfunc.o: prfunc.f90 status_module.mod profile_conf_interval_module.mod
	$(COMPILER) $(COPTIONS) prfunc.f90
gaussquad.o: gaussquad.f90 status_module.mod
	$(COMPILER) $(COPTIONS) gaussquad.f90
	
clean:
	rm -f *.o *.mod $(EXECUTABLE)
