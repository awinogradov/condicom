# Conditional React Component

Tiny helper which allows you to apply component enhancements by condition.

## Install

> npm i -S conditional-component

## Usage

Simple CSS only enhancement. For ex. you have component `Link` with some basic styles. 

`Link.tsx`
``` tsx
import * as React from 'react';

import './Link.css'; // file with default theme

export interface ILinkProps {
    url: string;
    text: React.ReactNode;
    theme?: 'default' | 'colorful';
}

export const Link: React.SFC<ILinkProps> = props => (
    <a className={`Link Link_theme_${props.theme}`} url={props.url}>{props.text}</a>
);

Link.defaultProps = {
    theme: 'default';
};
```

But, also, you have theme for this component. Of couse you don't want to have all of this CSS on the every page.

`Link_theme_coloful.tsx`
``` tsx
import { withCondition, matchProps } from 'conditional-component';

import { ILinkProps } from './Link';

import './Link_theme_colorful.css'; // file with extra theme

export const LinkThemeColorful = withCondition<ILinkProps>(matchProps({ theme: 'colorful' }));
```

Then in Project 1.

`index.tsx`
``` tsx
import { Link } from 'my-conditional-components/Link';

const Form = () => (
    <div className="Form">
        <Link url="#">Default</Link>
    </div>
);
```

And in Project 2.

`index.tsx`
``` tsx
import { Link } from 'my-conditional-components/Link';
import { LinkThemeColorful } from 'my-conditional-components/Link_theme_colorful';
import { compose } from 'really-typed-compose';

const EnhacedLink = compose(LinkThemeColorful)(Link);

// IT'LL BE APPLIED ONLY WITH MATCHED PROPS
const Form = () => (
    <div className="Form">
        <EnhacedLink theme="colorful" url="#">Colorful</EnhacedLink>
        <EnhacedLink url="#">Default</EnhacedLink>
    </div>
);
```

## NOT FOR CSS ONLY

You can make any compositions by conditions.

`Link_pseudo.tsx`
``` tsx
import { withCondition, matchProps } from 'conditional-component';

import { ILinkProps } from './Link';

import './Link_pseudo.css';

export const LinkThemeColorful = withCondition<ILinkProps>(
    matchProps({ pseudo: true }), 
    (Link, props) => (
        <div className="Wrapper">
            <Link {...props}>
                <span className="Link-Inner">{props.text}</span>
            </Link>
        </div>
    )
);
```

In Project 3.

`index.tsx`
``` tsx
import { Link } from 'my-conditional-components/Link';
import { LinkThemeColorful } from 'my-conditional-components/Link_theme_colorful';
import { LinkPseudo } from 'my-conditional-components/Link_pseudo';
import { compose } from 'really-typed-compose';

const EnhacedLink = compose(LinkThemeColorful, LinkPseudo)(Link);

// IT'LL APPLY ALL ENHANCEMENTS
const Form = () => (
    <div className="Form">
        <EnhacedLink theme="colorful" action url="#">
            ColorfulWrappedWithInner
        </EnhacedLink>
    </div>
);
```

### License [MIT](LICENSE)
