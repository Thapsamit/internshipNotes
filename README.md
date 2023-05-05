


# Chakra UI notes:-


## Layouts:-
- Box like a wrapper
- Container to contain some text
- Flex 
- grid



``` js

import { Input, InputGroup, InputLeftElement, Icon } from "@chakra-ui/react";
import { SearchIcon } from "@chakra-ui/icons";

function MyComponent() {
  return (
    <InputGroup>
      <InputLeftElement
        pointerEvents="none"
        children={<SearchIcon color="gray.300" />}
      />
      <Input type="text" placeholder="Search" />
    </InputGroup>
  );
}

```





























