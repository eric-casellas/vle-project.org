+++
topics = ["exmaple"]
tags = ["vle-1.1", "examples", "atomic-model", "counter"]
title = "counter"
+++

A simple model that counts events received in any input port.

First, we create a new package with the following command:

```bash
mkdir -p cd $HOME/vle/tutorial
cd $HOME/vle/tutorial
vle -P examples create
```

We add the file "counter.cpp" and update the `CMakeLists.txt` file in the `src`
directory.

```C++
#include <vle/devs/Dynamics.hpp>
#include <vle/value/Double.hpp>

namespace example {

class Counter : public vle::devs::Dynamics
{
public:
    Counter(const vle::devs::DynamicsInit& model,
            const vle::devs::InitEventList& events)
        : vle::devs::Dynamics(model, events)
        , m_counter(0),
    {}

    virtual ~Counter()
    {}

    virtual vle::devs::Time init(
                const vle::devs::Time& /* time */)
    {
        return 0;
    }

    virtual vle::devs::Time timeAdvance() const
    {
        return vle::devs::infinity;
    }

    virtual void externalTransition(
                const vle::devs::ExternalEventList& events,
                const vle::devs::Time& time)
    {
        m_counter += events.size();
    }

    virtual vle::value::Value* observation(
                const vle::devs::ObservationEvent& event) const
    {
        return new vle::value::Double(m_counter);
    }

private:
    std::size_t m_counter;
};

}}} // namespace vle examples counter

DECLARE_DYNAMICS(vle::example::Counter)
```

Then build the package:

```bash
    vle -P example configure build
```
