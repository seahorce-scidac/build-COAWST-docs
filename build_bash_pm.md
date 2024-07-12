1. SSH into perlmutter, check default modules 
```
ml
Currently Loaded Modules:
  1) craype-x86-milan     4) xpmem/2.6.2-2.5_2.38__gd067c3f.shasta   7) cray-libsci/23.12.5  10) gcc-native/12.3         13) cudatoolkit/12.2
  2) libfabric/1.15.2.0   5) PrgEnv-gnu/8.5.0                        8) cray-mpich/8.1.28    11) perftools-base/23.12.0  14) craype-accel-nvidia80
  3) craype-network-ofi   6) cray-dsmml/0.2.2                        9) craype/2.7.30        12) cpe/23.12               15) gpu/1.0

```
2. Create a module script needed for the compilation and run it 
```
vi perl_env_bash.sh
    module load cray-hdf5
    module load cray-netcdf
    module load cray-parallel-netcdf/1.12.3.9

source perl_env_bash.sh

ml
Currently Loaded Modules:
  1) craype-x86-milan     4) xpmem/2.6.2-2.5_2.38__gd067c3f.shasta   7) cray-libsci/23.12.5  10) gcc-native/12.3         13) cudatoolkit/12.2       16) cray-hdf5/1.12.2.9            (io)
  2) libfabric/1.15.2.0   5) PrgEnv-gnu/8.5.0                        8) cray-mpich/8.1.28    11) perftools-base/23.12.0  14) craype-accel-nvidia80  17) cray-netcdf/4.9.0.9           (io)
  3) craype-network-ofi   6) cray-dsmml/0.2.2                        9) craype/2.7.30        12) cpe/23.12               15) gpu/1.0                18) cray-parallel-netcdf/1.12.3.9 (io)

  Where:
   io:  Input/output software

```
3. Clone ```MCT``` for the coupler and go to that directory
```
git clone --recursive https://github.com/MCSclimate/MCT.git
cd MCT
```
4. Install ```MCT```. This follows help from the NERSC support team and steps are not explained in detail.
```
./configure --prefix=$SCRATCH/MCT/Install

checking for cc... cc
checking whether the C compiler works... yes
checking for C compiler default output file name... a.out
checking for suffix of executables... 
checking whether we are cross compiling... no
checking for suffix of object files... o
checking whether the compiler supports GNU C... yes
checking whether cc accepts -g... yes
checking for cc option to enable C11 features... none needed
checking for stdio.h... yes
checking for stdlib.h... yes
checking for string.h... yes
checking for inttypes.h... yes
checking for stdint.h... yes
checking for strings.h... yes
checking for sys/stat.h... yes
checking for sys/types.h... yes
checking for unistd.h... yes
checking whether byte ordering is bigendian... no
checking for nagfor... no
checking for xlf95... no
checking for pgf95... no
checking for ifort... no
checking for gfortran... gfortran
checking whether the compiler supports GNU Fortran... yes
checking whether gfortran accepts -g... yes
checking for Fortran flag to compile .F90 files... none
checking for mpif90... mpif90
checking for MPI_Init... yes
checking for mpif.h... yes
Checking Compiler Version
checking how to get the version output from gfortran... --version
checking for Fortran flag to compile preprocessed .F files... none
checking how to define symbols for preprocessed Fortran... -D
checking build system type... x86_64-pc-linux-gnu
checking host system type... x86_64-pc-linux-gnu
checking how to get verbose linking output from gfortran... -v
checking for Fortran libraries of gfortran...  -L/usr/lib64/gcc/x86_64-suse-linux/12 -L/usr/lib64/gcc/x86_64-suse-linux/12/../../../../lib64 -L/lib/../lib64 -L/usr/lib/../lib64 -L/usr/lib64/gcc/x86_64-suse-linux/12/../../../../x86_64-suse-linux/lib -L/usr/lib64/gcc/x86_64-suse-linux/12/../../.. -lgfortran -lm -lquadmath
checking for dummy main to link with Fortran libraries... none
checking for Fortran name-mangling scheme... lower case, underscore, no extra underscore
Hostname=login20
Machine=x86_64
OS=Linux
Fortran Compiler is GNU
checking if Fortran compiler supports allow-mismatch flag... yes
checking if Fortran compiler supports mismatch_all flag... no
checking for ranlib... ranlib

Output Variables: {CC=cc} {CFLAGS=-g -O2} {FC=gfortran} {FCFLAGS= -fallow-argument-mismatch} {PROGFCFLAGS=}{CPPDEFS= -DSYSLINUX -DCPRGNU} {OPT=-O2} {DEBUG=} {REAL8=} {BIT64=} {ENDIAN=} {MPIFC=mpif90} {MPILIBS=} {MPIHEADER=} {INCLUDEFLAG=-I} {INCLUDEPATH=} {AR=ar cq} {RANLIB=ranlib} {BABELROOT=} {COMPILER_ROOT=} {PYTHON=} {PYTHONOPTS=} {FORT_SIZE=} {prefix=/pscratch/sd/d/dylan617/MCT/Install} {SRCDIR=} {FC_DEFINE=-D}

configure: creating ./config.status
config.status: creating Makefile.conf
config.status: creating config.h
Please check the Makefile.conf
Have a nice day!
```
5. First ```make``` step.
```
make -j 8
make[1]: Entering directory '/pscratch/sd/d/dylan617/MCT/mpeu'
cc -c -DSYSLINUX -DCPRGNU -g -O2  -I. -I../ get_zeits.c
mpif90 -c  -I. -I../ -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch   m_mpif.F90
mpif90 -c  -I. -I../ -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch   m_realkinds.F90
mpif90 -c  -I. -I../ -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch   m_stdio.F90
mpif90 -c  -I. -I../ -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch   m_mpif90.F90
mpif90 -c  -I. -I../ -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch   m_dropdead.F90
mpif90 -c  -I. -I../ -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch   m_chars.F90
mpif90 -c  -I. -I../ -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch   m_flow.F90
mpif90 -c  -I. -I../ -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch   m_ioutil.F90
mpif90 -c  -I. -I../ -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch   m_mpout.F90
mpif90 -c  -I. -I../ -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch   m_die.F90
mpif90 -c  -I. -I../ -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch   m_IndexBin_char.F90
mpif90 -c  -I. -I../ -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch   m_IndexBin_integer.F90
mpif90 -c  -I. -I../ -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch   m_IndexBin_logical.F90
mpif90 -c  -I. -I../ -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch   m_mall.F90
mpif90 -c  -I. -I../ -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch   m_String.F90
m_String.F90:478:17:

  457 |   call MPI_bcast(ln,1,MP_INTEGER,root,comm,ier)
      |                 2
......
  478 |   call MPI_bcast(Str%c(1),ln,MP_CHARACTER,root,comm,ier)
      |                 1
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (CHARACTER(1)/INTEGER(4)).
mpif90 -c  -I. -I../ -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch   m_List.F90
m_List.F90:1945:15:

 1927 |  call MPI_RECV(length, 1, MP_type(length), source, TagBase, comm, &
      |               2
......
 1945 |  call MPI_RECV(DummStr%c(1), length, MP_CHARACTER, source, TagBase+1, &
      |               1
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (CHARACTER(1)/INTEGER(4)).
m_List.F90:1846:15:

 1831 |  call MPI_SEND(length, 1, MP_type(length), dest, TagBase, comm, ierr)
      |               2
......
 1846 |  call MPI_SEND(DummStr%c(1), length, MP_CHARACTER, dest, TagBase+1, &
      |               1
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (CHARACTER(1)/INTEGER(4)).
mpif90 -c  -I. -I../ -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch   m_MergeSorts.F90
mpif90 -c  -I. -I../ -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch   m_Filename.F90
mpif90 -c  -I. -I../ -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch   m_FcComms.F90
m_FcComms.F90:597:26:

  576 |                 call mpi_send ( signal, 1, recvtype, p, mtag, comm, ier )
      |                                2
......
  597 |           call mpi_rsend ( sendbuf, sendcnt, sendtype, root, mtag, &
      |                          1
Warning: Rank mismatch between actual argument at (1) and actual argument at (2) (scalar and rank-1)
m_FcComms.F90:430:33:

  430 |                 call mpi_irecv ( recvbuf(displs(q)+1), recvcnts(q), &
      |                                 1
......
  573 |                 call mpi_irecv ( recvbuf(displs(q)+1), recvcnts(q), &
      |                                 2
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (INTEGER(4)/REAL(8)).
m_FcComms.F90:433:32:

  433 |                 call mpi_send ( signal, 1, recvtype, p, mtag, comm, ier )
      |                                1
......
  576 |                 call mpi_send ( signal, 1, recvtype, p, mtag, comm, ier )
      |                                2
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (INTEGER(4)/REAL(8)).
m_FcComms.F90:452:26:

  452 |           call mpi_recv ( signal, 1, sendtype, root, mtag, comm, &
      |                          1
......
  595 |           call mpi_recv ( signal, 1, sendtype, root, mtag, comm, &
      |                          2
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (INTEGER(4)/REAL(8)).
m_FcComms.F90:465:22:

  465 |     call mpi_gatherv (sendbuf, sendcnt, sendtype, &
      |                      1
......
  608 |     call mpi_gatherv (sendbuf, sendcnt, sendtype, &
      |                      2
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (INTEGER(4)/REAL(8)).
m_FcComms.F90:466:22:

  466 |                       recvbuf, recvcnts, displs, recvtype, &
      |                      1
......
  609 |                       recvbuf, recvcnts, displs, recvtype, &
      |                      2
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (INTEGER(4)/REAL(8)).
m_FcComms.F90:312:26:

  312 |           call mpi_rsend ( sendbuf, sendcnt, sendtype, root, mtag, &
      |                          1
......
  576 |                 call mpi_send ( signal, 1, recvtype, p, mtag, comm, ier )
      |                                2
Warning: Rank mismatch between actual argument at (1) and actual argument at (2) (scalar and rank-1)
m_FcComms.F90:181:21:

  181 |     call mpi_gather (sendbuf, sendcnt, sendtype, &
      |                     1
......
  323 |     call mpi_gather (sendbuf, sendcnt, sendtype, &
      |                     2
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (INTEGER(4)/REAL(8)).
m_FcComms.F90:182:21:

  182 |                      recvbuf, recvcnt, recvtype, &
      |                     1
......
  324 |                      recvbuf, recvcnt, recvtype, &
      |                     2
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (INTEGER(4)/REAL(8)).
mpif90 -c  -I. -I../ -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch   m_Permuter.F90
mpif90 -c  -I. -I../ -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch   m_rankMerge.F90
mpif90 -c  -I. -I../ -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch   m_SortingTools.F90
mpif90 -c  -I. -I../ -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch   m_StrTemplate.F90
mpif90 -c  -I. -I../ -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch   m_FileResolv.F90
mpif90 -c  -I. -I../ -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch   m_inpak90.F90
m_inpak90.F90:369:17:

  362 |   call MPI_Bcast(i90_now%buffer,NBUF_MAX,MP_CHARACTER,root,comm,ier)
      |                 2
......
  369 |   call MPI_Bcast(i90_now%nbuf,1,MP_INTEGER,root,comm,ier)
      |                 1
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (INTEGER(4)/CHARACTER(*)).
mpif90 -c  -I. -I../ -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch   m_TraceBack.F90
mpif90 -c  -I. -I../ -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch   m_zeit.F90
rm -f libmpeu.a
ar cq libmpeu.a get_zeits.o m_IndexBin_char.o m_IndexBin_integer.o m_IndexBin_logical.o m_List.o m_MergeSorts.o m_Filename.o m_FcComms.o m_Permuter.o m_SortingTools.o m_String.o m_StrTemplate.o m_chars.o m_die.o m_dropdead.o m_FileResolv.o m_flow.o m_inpak90.o m_ioutil.o m_mall.o m_mpif.o m_mpif90.o m_mpout.o m_rankMerge.o m_realkinds.o m_stdio.o m_TraceBack.o m_zeit.o
ranlib libmpeu.a
make[1]: Leaving directory '/pscratch/sd/d/dylan617/MCT/mpeu'
make[1]: Entering directory '/pscratch/sd/d/dylan617/MCT/mct'
mpif90 -c  -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch  -I/pscratch/sd/d/dylan617/MCT/mpeu m_MCTWorld.F90
m_MCTWorld.F90:335:22:

  266 |         call MPI_SEND(mysize,1,MP_INTEGER,0,myids(i),globalcomm,ier)
      |                      2
......
  335 |         call MPI_SEND(Gprocids,mysize,MP_INTEGER,0,myids(i),globalcomm,ier)
      |                      1
Warning: Rank mismatch between actual argument at (1) and actual argument at (2) (scalar and rank-1)
mpif90 -c  -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch  -I/pscratch/sd/d/dylan617/MCT/mpeu m_AttrVect.F90
mpif90 -c  -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch  -I/pscratch/sd/d/dylan617/MCT/mpeu m_GlobalMap.F90
m_GlobalMap.F90:389:17:

  362 |   call MPI_bcast(nPEs, 1, MP_INTEGER, my_root, my_comm, ier)
      |                 2
......
  389 |   call MPI_bcast(GMap%counts, nPEs, MP_INTEGER, my_root, my_comm, ier)
      |                 1
Warning: Rank mismatch between actual argument at (1) and actual argument at (2) (scalar and rank-1)
m_GlobalMap.F90:265:17:

  265 |   call MPI_bcast(GMap%counts, nPEs, MP_INTEGER, root, comm, ier)
      |                 1
......
  362 |   call MPI_bcast(nPEs, 1, MP_INTEGER, my_root, my_comm, ier)
      |                 2
Warning: Rank mismatch between actual argument at (1) and actual argument at (2) (scalar and rank-1)
mpif90 -c  -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch  -I/pscratch/sd/d/dylan617/MCT/mpeu m_GlobalSegMap.F90
m_GlobalSegMap.F90:562:17:

  528 |   call MPI_BCAST(GSMap%ngseg, 1, MP_INTEGER, root, my_comm, ier)
      |                 2
......
  562 |   call MPI_BCAST(GSMap%start, GSMap%ngseg, MP_INTEGER, root, my_comm, ier)
      |                 1
Warning: Rank mismatch between actual argument at (1) and actual argument at (2) (scalar and rank-1)
m_GlobalSegMap.F90:565:17:

  528 |   call MPI_BCAST(GSMap%ngseg, 1, MP_INTEGER, root, my_comm, ier)
      |                 2
......
  565 |   call MPI_BCAST(GSMap%length, GSMap%ngseg, MP_INTEGER, root, my_comm, ier)
      |                 1
Warning: Rank mismatch between actual argument at (1) and actual argument at (2) (scalar and rank-1)
m_GlobalSegMap.F90:568:17:

  528 |   call MPI_BCAST(GSMap%ngseg, 1, MP_INTEGER, root, my_comm, ier)
      |                 2
......
  568 |   call MPI_BCAST(GSMap%pe_loc, GSMap%ngseg, MP_INTEGER, root, my_comm, ier)
      |                 1
Warning: Rank mismatch between actual argument at (1) and actual argument at (2) (scalar and rank-1)
mpif90 -c  -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch  -I/pscratch/sd/d/dylan617/MCT/mpeu m_GlobalSegMapComms.F90
m_GlobalSegMapComms.F90:258:17:

  238 |   call MPI_ISEND(outgoingGSMap%comp_id, 1, MP_Type(outgoingGSMap%comp_id), destID, &
      |                 2
......
  258 |   call MPI_ISEND(outgoingGSMap%start, nsegs, &
      |                 1
Warning: Rank mismatch between actual argument at (1) and actual argument at (2) (scalar and rank-1)
m_GlobalSegMapComms.F90:265:17:

  238 |   call MPI_ISEND(outgoingGSMap%comp_id, 1, MP_Type(outgoingGSMap%comp_id), destID, &
      |                 2
......
  265 |   call MPI_ISEND(outgoingGSMap%length, nsegs, &
      |                 1
Warning: Rank mismatch between actual argument at (1) and actual argument at (2) (scalar and rank-1)
m_GlobalSegMapComms.F90:272:17:

  238 |   call MPI_ISEND(outgoingGSMap%comp_id, 1, MP_Type(outgoingGSMap%comp_id), destID, &
      |                 2
......
  272 |   call MPI_ISEND(outgoingGSMap%pe_loc, nsegs, &
      |                 1
Warning: Rank mismatch between actual argument at (1) and actual argument at (2) (scalar and rank-1)
m_GlobalSegMapComms.F90:133:16:

  112 |   call MPI_SEND(outgoingGSMap%comp_id, 1, MP_Type(outgoingGSMap%comp_id), destID, &
      |                2
......
  133 |   call MPI_SEND(outgoingGSMap%start, nsegs, &
      |                1
Warning: Rank mismatch between actual argument at (1) and actual argument at (2) (scalar and rank-1)
m_GlobalSegMapComms.F90:140:16:

  112 |   call MPI_SEND(outgoingGSMap%comp_id, 1, MP_Type(outgoingGSMap%comp_id), destID, &
      |                2
......
  140 |   call MPI_SEND(outgoingGSMap%length, nsegs, &
      |                1
Warning: Rank mismatch between actual argument at (1) and actual argument at (2) (scalar and rank-1)
m_GlobalSegMapComms.F90:147:16:

  112 |   call MPI_SEND(outgoingGSMap%comp_id, 1, MP_Type(outgoingGSMap%comp_id), destID, &
      |                2
......
  147 |   call MPI_SEND(outgoingGSMap%pe_loc, nsegs, &
      |                1
Warning: Rank mismatch between actual argument at (1) and actual argument at (2) (scalar and rank-1)
mpif90 -c  -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch  -I/pscratch/sd/d/dylan617/MCT/mpeu m_Accumulator.F90
mpif90 -c  -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch  -I/pscratch/sd/d/dylan617/MCT/mpeu m_AttrVectComms.F90
m_AttrVectComms.F90:1657:20:

 1570 |   call MPI_bcast(nIA,1,MP_INTEGER,root,comm,ier)
      |                 2   
......
 1657 |      call MPI_bcast(aV%iAttr,nIA*lsize,mp_Type_aV,root,comm,ier)
      |                    1
Warning: Rank mismatch between actual argument at (1) and actual argument at (2) (scalar and rank-2)
m_AttrVectComms.F90:1669:20:

 1570 |   call MPI_bcast(nIA,1,MP_INTEGER,root,comm,ier)
      |                 2   
......
 1669 |      call MPI_bcast(aV%rAttr,nRA*lsize,mp_Type_aV,root,comm,ier)
      |                    1
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (REAL(8)/INTEGER(4)).
m_AttrVectComms.F90:1070:26:

 1041 |         call MPI_scatterv(iV%iAttr,GMap%counts*nIA, &
      |                          2
......
 1070 |         call MPI_scatterv(iV%rAttr,GMap%counts*nRA, &
      |                          1
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (REAL(8)/INTEGER(4)).
m_AttrVectComms.F90:1071:40:

 1042 |              GMap%displs*nIA,MP_INTEGER,oV%iAttr,   &
      |                                        2
......
 1071 |              GMap%displs*nRA,mp_type_Av,oV%rAttr,   &
      |                                        1
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (REAL(8)/INTEGER(4)).
m_AttrVectComms.F90:356:16:

  319 |   call MPI_RECV(ListAssoc, 2, MP_LOGICAL, dest, TagBase, comm, &
      |                2
......
  356 |   call MPI_RECV(AVlength, 1, MP_type(AVlength), dest, TagBase+5, &
      |                1
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (INTEGER(4)/LOGICAL(4)).
m_AttrVectComms.F90:206:16:

  163 |   call MPI_SEND(ListAssoc, 2, MP_LOGICAL, dest, TagBase, comm, ierr)
      |                2
......
  206 |   call MPI_SEND(AVlength, 1, MP_type(AVlength), dest, TagBase+5, &
      |                1
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (INTEGER(4)/LOGICAL(4)).
mpif90 -c  -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch  -I/pscratch/sd/d/dylan617/MCT/mpeu m_SparseMatrix.F90
m_SparseMatrix.F90:2380:21:

 2350 |   call MPI_ALLREDUCE(end_row, num_rows, 1, MP_INTEGER, MP_MAX, &
      |                     2
......
 2380 |   call MPI_ALLREDUCE(lsums, gsums, num_rows, mp_Type_lsums, MP_SUM, comm, ierr)
      |                     1
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (REAL(8)/INTEGER(4)).
m_SparseMatrix.F90:2380:28:

 2350 |   call MPI_ALLREDUCE(end_row, num_rows, 1, MP_INTEGER, MP_MAX, &
      |                              2
......
 2380 |   call MPI_ALLREDUCE(lsums, gsums, num_rows, mp_Type_lsums, MP_SUM, comm, ierr)
      |                            1
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (REAL(8)/INTEGER(4)).
mpif90 -c  -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch  -I/pscratch/sd/d/dylan617/MCT/mpeu m_Navigator.F90
mpif90 -c  -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch  -I/pscratch/sd/d/dylan617/MCT/mpeu m_AttrVectReduce.F90
m_AttrVectReduce.F90:551:20:

  519 |         call MPI_AllReduce(inAV%rAttr, outAV%rAttr, BufferSize, &
      |                           2
......
  551 |         call MPI_AllReduce(inAV%iAttr, outAV%iAttr, BufferSize, &
      |                           1
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (INTEGER(4)/REAL(8)).
m_AttrVectReduce.F90:551:32:

  519 |         call MPI_AllReduce(inAV%rAttr, outAV%rAttr, BufferSize, &
      |                                       2
......
  551 |         call MPI_AllReduce(inAV%iAttr, outAV%iAttr, BufferSize, &
      |                                       1
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (INTEGER(4)/REAL(8)).
mpif90 -c  -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch  -I/pscratch/sd/d/dylan617/MCT/mpeu m_AccumulatorComms.F90
m_AccumulatorComms.F90:713:17:

  677 |   call MPI_BCAST(AccBuffSize, 1, MP_INTEGER, root, comm, ier)
      |                 2
......
  713 |   call MPI_BCAST(AccBuff, AccBuffSize, MP_INTEGER, root, comm, ier)
      |                 1
Warning: Rank mismatch between actual argument at (1) and actual argument at (2) (scalar and rank-1)
mpif90 -c  -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch  -I/pscratch/sd/d/dylan617/MCT/mpeu m_GeneralGrid.F90
mpif90 -c  -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch  -I/pscratch/sd/d/dylan617/MCT/mpeu m_GeneralGridComms.F90
m_GeneralGridComms.F90:1369:20:

 1281 |   call MPI_BCAST(HeaderAssoc,6,MP_LOGICAL,root,comm,ierr)
      |                 2   
......
 1369 |      call MPI_BCAST(DescendSize, 1, MP_INTEGER, root, comm, ierr)
      |                    1
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (INTEGER(4)/LOGICAL(4)).
m_GeneralGridComms.F90:476:19:

  424 |   call MPI_RECV(HeaderAssoc, 6, MP_LOGICAL, source, TagBase, ThisMCTWorld%MCT_comm, MPstatus, ierr)
      |                2   
......
  476 |      call MPI_RECV(DescendSize, 1, MP_type(DescendSize), &
      |                   1
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (INTEGER(4)/LOGICAL(4)).
m_GeneralGridComms.F90:210:19:

  156 |   call MPI_SEND(HeaderAssoc, 6, MP_LOGICAL, dest, TagBase, ThisMCTWorld%MCT_comm, ierr)
      |                2   
......
  210 |      call MPI_SEND(size(iGGrid%descend), 1, MP_type(size(iGGrid%descend)), &
      |                   1
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (INTEGER(4)/LOGICAL(4)).
mpif90 -c  -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch  -I/pscratch/sd/d/dylan617/MCT/mpeu m_SpatialIntegralV.F90
mpif90 -c  -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch  -I/pscratch/sd/d/dylan617/MCT/mpeu m_SpatialIntegral.F90
mpif90 -c  -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch  -I/pscratch/sd/d/dylan617/MCT/mpeu m_GlobalToLocal.F90
mpif90 -c  -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch  -I/pscratch/sd/d/dylan617/MCT/mpeu m_ConvertMaps.F90
mpif90 -c  -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch  -I/pscratch/sd/d/dylan617/MCT/mpeu m_ExchangeMaps.F90
mpif90 -c  -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch  -I/pscratch/sd/d/dylan617/MCT/mpeu m_Router.F90
mpif90 -c  -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch  -I/pscratch/sd/d/dylan617/MCT/mpeu m_SPMDutils.F90
m_SPMDutils.F90:794:28:

  723 |       call mpi_irecv( rcvbuf(offset_r), rcvlths(mytask), rtypes(mytask), &
      |                      2      
......
  794 |             call mpi_irecv( hs, 1, MP_INTEGER, p, tag, comm, &
      |                            1
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (INTEGER(4)/REAL(8)).
m_SPMDutils.F90:811:28:

  728 |       call mpi_send( sndbuf(offset_s), sndlths(mytask), stypes(mytask), &
      |                     2       
......
  811 |             call mpi_send ( hs, 1, MP_INTEGER, p, tag, comm, ier )
      |                            1
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (INTEGER(4)/REAL(8)).
m_SPMDutils.F90:402:28:

  402 |             call mpi_irsend( sndbuf(offset_s), sndlths(p), stypes(p), &
      |                            1
......
  928 |             call mpi_irsend( sndbuf(offset_s), sndlths(p), stypes(p), &
      |                            2
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (INTEGER(4)/REAL(8)).
mpif90 -c  -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch  -I/pscratch/sd/d/dylan617/MCT/mpeu m_Rearranger.F90
m_Rearranger.F90:981:19:

  954 |            call MPI_IRECV(IRecvBuf(IRecvLoc(proc)),                 &
      |                          2
......
  981 |            call MPI_IRECV(RRecvBuf(RRecvLoc(proc)),                 &
      |                          1
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (REAL(8)/INTEGER(4)).
m_Rearranger.F90:1069:19:

 1042 |            call MPI_ISEND(ISendBuf(ISendLoc(proc)),                 &
      |                          2
......
 1069 |            call MPI_ISEND(RSendBuf(RSendLoc(proc)),                 &
      |                          1
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (REAL(8)/INTEGER(4)).
m_Rearranger.F90:1172:24:

 1166 |      call MPI_Alltoallv(ISendBuf, ISendCnts, ISdispls, MP_INTEGER, &
      |                        2
......
 1172 |      call MPI_Alltoallv(RSendBuf, RSendCnts, RSdispls, mp_Type_rp, &
      |                        1
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (REAL(8)/INTEGER(4)).
m_Rearranger.F90:1173:24:

 1167 |                         IRecvBuf, IRecvCnts, IRdispls, MP_INTEGER, &
      |                        2
......
 1173 |                         RRecvBuf, RRecvCnts, RRdispls, mp_Type_rp, &
      |                        1
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (REAL(8)/INTEGER(4)).
mpif90 -c  -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch  -I/pscratch/sd/d/dylan617/MCT/mpeu m_SparseMatrixDecomp.F90
mpif90 -c  -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch  -I/pscratch/sd/d/dylan617/MCT/mpeu m_SparseMatrixComms.F90
mpif90 -c  -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch  -I/pscratch/sd/d/dylan617/MCT/mpeu m_SparseMatrixToMaps.F90
mpif90 -c  -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch  -I/pscratch/sd/d/dylan617/MCT/mpeu m_SparseMatrixPlus.F90
mpif90 -c  -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch  -I/pscratch/sd/d/dylan617/MCT/mpeu m_MatAttrVectMul.F90
mpif90 -c  -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch  -I/pscratch/sd/d/dylan617/MCT/mpeu m_Merge.F90
mpif90 -c  -DSYSLINUX -DCPRGNU -O2   -fallow-argument-mismatch  -I/pscratch/sd/d/dylan617/MCT/mpeu m_Transfer.F90
m_Transfer.F90:551:19:

  526 |            call MPI_IRECV(Rout%ip1(proc)%pi(1), &
      |                          2
......
  551 |            call MPI_IRECV(Rout%rp1(proc)%pr(1), &
      |                          1
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (REAL(8)/INTEGER(4)).
m_Transfer.F90:256:22:

  240 |         call MPI_ISEND(Rout%ip1(proc)%pi(1), &
      |                       2
......
  256 |        call MPI_ISEND(Rout%rp1(proc)%pr(1), &
      |                      1
Warning: Type mismatch between actual argument at (1) and actual argument at (2) (REAL(8)/INTEGER(4)).
rm -f libmct.a
ar cq libmct.a m_MCTWorld.o m_AttrVect.o m_GlobalMap.o m_GlobalSegMap.o m_GlobalSegMapComms.o m_Accumulator.o m_SparseMatrix.o m_Navigator.o m_AttrVectComms.o m_AttrVectReduce.o m_AccumulatorComms.o m_GeneralGrid.o m_GeneralGridComms.o m_SpatialIntegral.o m_SpatialIntegralV.o m_MatAttrVectMul.o m_Merge.o m_GlobalToLocal.o m_ExchangeMaps.o m_ConvertMaps.o m_SparseMatrixDecomp.o m_SparseMatrixToMaps.o m_SparseMatrixComms.o m_SparseMatrixPlus.o m_Router.o m_Rearranger.o m_SPMDutils.o m_Transfer.o
ranlib libmct.a
make[1]: Leaving directory '/pscratch/sd/d/dylan617/MCT/mct'
```
6. Second ```make step```. 
```
make install
make[1]: Entering directory '/pscratch/sd/d/dylan617/MCT/mpeu'
make[1]: Nothing to be done for 'all'.
make[1]: Leaving directory '/pscratch/sd/d/dylan617/MCT/mpeu'
make[1]: Entering directory '/pscratch/sd/d/dylan617/MCT/mct'
make[1]: Nothing to be done for 'all'.
make[1]: Leaving directory '/pscratch/sd/d/dylan617/MCT/mct'
make[1]: Entering directory '/pscratch/sd/d/dylan617/MCT/mpeu'
/pscratch/sd/d/dylan617/MCT/mkinstalldirs /pscratch/sd/d/dylan617/MCT/Install/lib /pscratch/sd/d/dylan617/MCT/Install/include
mkdir -p -- /pscratch/sd/d/dylan617/MCT/Install/lib /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c libmpeu.a -m 644 /pscratch/sd/d/dylan617/MCT/Install/lib
/pscratch/sd/d/dylan617/MCT/install-sh -c m_chars.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_die.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_dropdead.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_fccomms.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_filename.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_fileresolv.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_flow.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_indexbin_char.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_indexbin_integer.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_indexbin_logical.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_inpak90.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_ioutil.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_list.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_mall.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_mergesorts.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_mpif90.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_mpif.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_mpout.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_permuter.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_rankmerge.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_realkinds.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_sortingtools.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_stdio.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_string.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_strtemplate.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_traceback.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_zeit.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
make[1]: Leaving directory '/pscratch/sd/d/dylan617/MCT/mpeu'
make[1]: Entering directory '/pscratch/sd/d/dylan617/MCT/mct'
/pscratch/sd/d/dylan617/MCT/mkinstalldirs /pscratch/sd/d/dylan617/MCT/Install/lib /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c libmct.a -m 644 /pscratch/sd/d/dylan617/MCT/Install/lib
/pscratch/sd/d/dylan617/MCT/install-sh -c m_accumulatorcomms.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_accumulator.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_attrvectcomms.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_attrvect.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_attrvectreduce.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_convertmaps.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_exchangemaps.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_generalgridcomms.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_generalgrid.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_globalmap.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_globalsegmapcomms.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_globalsegmap.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_globaltolocal.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_matattrvectmul.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_mctworld.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_merge.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_navigator.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_rearranger.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_router.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_sparsematrixcomms.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_sparsematrixdecomp.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_sparsematrix.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_sparsematrixplus.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_sparsematrixtomaps.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_spatialintegral.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_spatialintegralv.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_spmdutils.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
/pscratch/sd/d/dylan617/MCT/install-sh -c m_transfer.mod -m 644 /pscratch/sd/d/dylan617/MCT/Install/include
make[1]: Leaving directory '/pscratch/sd/d/dylan617/MCT/mct'
```
7. Export the MCT paths. The compilation *will* break if you do not do this. 
```
export MCT_INCDIR=$SCRATCH/MCT/Install/include
export MCT_LIBDIR=$SCRATCH/MCT/Install/lib 
```
8. Clone COAWST and ```cd``` to the directory if you have not installed it already. 
```
git clone --recursive -j8 https://github.com/DOI-USGS/COAWST.git
cd COAWST
```
9. ```vi build_coawst.sh``` (change line 141 to export MY_ROOT_DIR=${PWD})
10. ```vi Compilers/Linux-gfortran.mk``` (change lines 190 and 191)
```
NETCDF_INCDIR ?= $(NETCDF_DIR)/include
LIBS += -L$(NETCDF_DIR)/lib -lnetcdf -lnetcdff
```
11. Run the build script ```./build_coawst.sh```. If it is successful, a ```coawstM``` will be produced. I'm skipping providing the output of the compiler because it is thousands of lines long. 
