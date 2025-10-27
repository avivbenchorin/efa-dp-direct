NVCC=nvcc
NVCC_FLAGS=-G -rdc=true
CUDA_FLAGS=-Xcompiler -fPIC

SRCDIR=src
BUILDDIR=build
INCDIR=$(BUILDDIR)/include

all: $(BUILDDIR)/libefacudadp.so headers

$(BUILDDIR):
	mkdir -p $(BUILDDIR)

$(INCDIR): | $(BUILDDIR)
	mkdir -p $(INCDIR)

headers: $(INCDIR)
	cp $(SRCDIR)/*.h $(SRCDIR)/*.cuh $(INCDIR)/

$(BUILDDIR)/libefacudadp.so: $(BUILDDIR)/efa_cuda_dp.cu.o $(BUILDDIR)/efa_cuda_dp.o | $(BUILDDIR)
	$(NVCC) $(NVCC_FLAGS) -shared -o $@ $^

$(BUILDDIR)/efa_cuda_dp.cu.o: $(SRCDIR)/efa_cuda_dp.cu $(SRCDIR)/efa_cuda_dp.cuh $(SRCDIR)/efa_cuda_dp_impl.cuh | $(BUILDDIR)
	$(NVCC) $(NVCC_FLAGS) -c $< $(CUDA_FLAGS) -o $@

$(BUILDDIR)/efa_cuda_dp.o: $(SRCDIR)/efa_cuda_dp.cpp $(SRCDIR)/efa_cuda_dp.h $(SRCDIR)/efa_cuda_dp.cuh | $(BUILDDIR)
	$(NVCC) $(NVCC_FLAGS) -c $< $(CUDA_FLAGS) -o $@

clean:
	rm -rf $(BUILDDIR)
