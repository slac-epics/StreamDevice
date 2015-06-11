TOP=..

# Look if we have EPICS R3.13 or R3.14
ifeq ($(wildcard $(TOP)/configure),)
# EPICS R3.13
include $(TOP)/config/CONFIG_APP
# The real work is in Makefile.Vx
include $(TOP)/config/RULES_ARCHS
else

# EPICS R3.14
include $(TOP)/configure/CONFIG

#HOST_OPT = NO

PROD = streamApp
DBD  = streamApp.dbd

streamApp_DBD += base.dbd

# In 3.14.12 aaiRecord.dbd and aaoRecord.dbd are part of base.dbd
# In earlier versions, these records are buggy and not included by default
#streamApp_DBD += aaiRecord.dbd aaoRecord.dbd

PROD_SRCS += streamApp_registerRecordDeviceDriver.cpp
PROD_SRCS_DEFAULT = streamAppMain.cc
PROD_SRCS_vxWorks = -nil-

PROD_LIBS = stream

ifdef ASYN
# edit asynRegistrars.dbd if necessary
streamApp_DBD += asynRegistrars.dbd
# add asynRecord.dbd if you like
streamApp_DBD += asynRecord.dbd
PROD_LIBS += asyn
endif

ifneq ($(words $(CALC) $(SYNAPPS)), 0)
# With synApps scalcout record
streamApp_DBD += calcSupport.dbd
streamApp_DBD += streamSynApps.dbd
PROD_LIBS += calc
# older calc versions require sscan
#PROD_LIBS += sscan
PROD_LIBS_DEFAULT += streamSynApps
endif

streamApp_DBD += stream.dbd

pcre_DIR = $(PCRE_LIB)

ifneq ($(T_A),linuxRT-x86)
  ifneq ($(T_A),linuxRT-x86_64)
       PROD_LIBS_Linux += pcre
  endif
endif

PROD_LIBS += $(EPICS_BASE_IOC_LIBS)

# Write StreamDevice debug output to this file
# If unset output goes to stdout
CPPFLAGS += -DDEBUGFILE=StreamDebug.log

include $(TOP)/configure/RULES

ifeq ($(BASE_3_14), YES)
clean:: myclean
else
clean: myclean
endif

myclean:
	$(RM) core* StreamDebug.log

endif
