ENVS := $(filter-out all,$(wildcard overlays/*))
ENVS := $(notdir $(ENVS))
KUSTOMIZE_CMD = kustomize build
KUBECTL_APPLY = | kubectl apply -f -

preview-%:
	@if echo "$(ENVS)" | grep -wq "$*"; then \
		$(KUSTOMIZE_CMD) overlays/$*; \
	else \
		echo "Invalid environment: $*. Valid options are: $(ENVS)"; exit 1; \
	fi

apply-%:
	@if [ "$*" = "all" ]; then \
		for env in $(ENVS); do \
			$(KUSTOMIZE_CMD) overlays/$$env $(KUBECTL_APPLY); \
		done; \
	elif echo "$(ENVS)" | grep -wq "$*"; then \
		$(KUSTOMIZE_CMD) overlays/$* $(KUBECTL_APPLY); \
	else \
		echo "Invalid environment: $*. Valid options are: $(ENVS)"; exit 1; \
	fi

apply-all:
	@for env in $(ENVS); do \
		$(MAKE) apply-$$env; \
	done
