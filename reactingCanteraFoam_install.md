1. **加载 OpenFOAM/7-gcc**
```
module purge
module load openfoam/7-gcc
```

2. **安装 Cantera**

```
module load anaconda/3/2024.10.1
conda create --name ct-env --channel conda-forge cantera ipython matplotlib jupyter
conda activate ct-env
conda install -c cantera cantera 
conda install -c cantera libcantera-devel 
```

3. **安装[reactingCanteraFoam](https://github.com/ZhangYanTJU/reactingCanteraFoam)**
```
git clone https://github.com/ZhangYanTJU/reactingCanteraFoam.git
cd reactingCanteraFoam

修改Make/options
EXE_INC = \
    -I. \
    -I$(FOAM_APP)/solvers/combustion/reactingFoam \
    -I$(LIB_SRC)/finiteVolume/lnInclude \
    -I$(LIB_SRC)/meshTools/lnInclude \
    -I$(LIB_SRC)/sampling/lnInclude \
    -I$(LIB_SRC)/TurbulenceModels/turbulenceModels/lnInclude \
    -I$(LIB_SRC)/TurbulenceModels/compressible/lnInclude \
    -I$(LIB_SRC)/thermophysicalModels/specie/lnInclude \
    -I$(LIB_SRC)/thermophysicalModels/reactionThermo/lnInclude \
    -I$(LIB_SRC)/transportModels/compressible/lnInclude \
    -I$(LIB_SRC)/thermophysicalModels/basic/lnInclude \
    -I$(LIB_SRC)/thermophysicalModels/chemistryModel/lnInclude \
    -I$(LIB_SRC)/ODE/lnInclude \
    -I$(LIB_SRC)/combustionModels/lnInclude \
    -I$(CONDA_PREFIX)/include

EXE_LIBS = \
    -lfiniteVolume \
    -lfvOptions \
    -lmeshTools \
    -lsampling \
    -lturbulenceModels \
    -lcompressibleTurbulenceModels \
    -lreactionThermophysicalModels \
    -lspecie \
    -lcompressibleTransportModels \
    -lfluidThermophysicalModels \
    -lchemistryModel \
    -lODE \
    -lcombustionModels \
    -L$(CONDA_PREFIX)/lib \
    -lcantera_shared \
    -L$(CONDA_PREFIX)/lib \
    -lmkl_rt \
    -liomp5 \
    -lpthread \
    -lm \
    -ldl

export MKL_ROOT=$CONDA_PREFIX
export LD_LIBRARY_PATH=$CONDA_PREFIX/lib:$LD_LIBRARY_PATH
wclean
wmake

```

4. **测试reactingCanteraFoam**
```
cd testCase
export LD_LIBRARY_PATH=../cantera_build/lib:$LD_LIBRARY_PATH
export CANTERA_DATA=../cantera_build/data
reactingCanteraFoam
```
