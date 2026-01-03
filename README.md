# POSEIDON DEVELOPMENT - Credit Card Skimming Script

**Copyright (c) 2025 Poseidon Developments - All Rights Reserved**

A comprehensive FiveM script that allows criminals to skim credit cards from players/NPCs and inject them into ATMs to receive black money. Includes a fully integrated shop system with NPC vendors.

## Features

- ✅ **Credit Card Skimming**: Skim credit cards from nearby players and NPCs
- ✅ **ATM Injection**: Inject skimmed cards into ATMs to receive black money or bank money
- ✅ **Integrated Shop System**: Buy equipment from NPC vendors using ox_lib UI and ox_target
- ✅ **POSEIDON DEVELOPMENT Branding**: Full branding throughout codebase for copyright protection
- ✅ **Global Limits System**: Prevents 24/7 skimming with hourly and daily limits per player
- ✅ **Multiple Banking Systems**: Supports okok banking, qb banking, or black_money items (auto-detect or manual)
- ✅ **Local Caching**: Advanced caching system for improved performance and reduced database calls
- ✅ **Multiple Dispatch Support**: Compatible with rcore_dispatch, lb-phone, wasabi_dispatch, and cd_dispatch
- ✅ **Police Alerts**: Always alerts police on skimming attempts (configurable)
- ✅ **Ox Integration**: Full integration with ox_lib, ox_target, and ox_inventory
- ✅ **Highly Optimized**: Performance optimizations including distance caching, NPC caching, and efficient loops
- ✅ **Configurable**: Highly configurable success rates, cooldowns, limits, and amounts
- ✅ **Server-Side Validation**: All actions validated server-side for security
- ✅ **Anti-Exploit Protection**: Rate limiting, input validation, distance checks, and security logging
- ✅ **Dual Framework Support**: Works with both QBCore and QBox frameworks (auto-detected)

## Requirements

- QBCore OR QBox Framework (auto-detected)
- ox_lib (latest version)
- ox_target
- ox_inventory (or framework inventory)
- One of the following dispatch systems (optional):
  - rcore_dispatch
  - lb-phone (with dispatch)
  - wasabi_dispatch
  - cd_dispatch

## Installation

1. Download and place the script in your `resources` folder
2. Rename the folder to `cc-card-skimming` (or your preferred name)
3. Add the items from `items.lua` to your `qb-core/shared/items.lua`
4. Add item images to your inventory system
5. Add `ensure cc-card-skimming` (or your folder name) to your `server.cfg`
6. Restart your server

## Configuration

Edit `config.lua` to customize:

- **Skimming Settings**: Duration, cooldown, success chance, alert chance, hourly/daily limits
- **ATM Settings**: Duration, cooldown, success chance, min/max money amounts, quality alert multipliers
- **Card Quality System**: Configure tiers, value ranges, success chances, and spawn weights
- **Item Decay Settings**: Configure decay times for cards (in seconds), warning times
- **Global Limits**: Max cards per hour/day to prevent 24/7 skimming
- **Banking System**: Auto-detect or manually set (okok, qb, or item)
- **Dispatch System**: Auto-detect or manually set your dispatch system
- **Police Alerts**: Configure when police are alerted (always, on success, on failure)
- **Items**: Customize item names if needed
- **Notifications**: Customize all notification messages

### Banking System Configuration

The script supports multiple banking systems:

```lua
Config.Banking = {
    System = 'auto', -- 'auto', 'okok', 'qb', 'item'
    AccountType = 'savings', -- For okok: 'savings' or 'checking'
    UseBlackMoneyItem = false, -- Force black_money item even if banking system exists
}
```

- **auto**: Automatically detects which banking system is installed
- **okok**: Uses okokBanking system (adds money to bank account)
- **qb**: Uses qb-banking or QBCore banking (adds money to bank account)
- **item**: Uses black_money item (default if no banking system found)

## Items Required

The script uses the following items:

1. **card_skimmer** - Device needed to skim credit cards
2. **credit_card** - Skimmed credit card (obtained from skimming)
3. **card_reader** - Device needed to inject cards into ATMs
4. **black_money** - Received after successful ATM injection

## Shop System

The script includes a fully integrated shop system where players can purchase skimming equipment:

1. **Shop Locations**: Configure shop locations in `config.lua` under `Config.Shop.Locations`
2. **Find a Shop**: Look for the "Illegal Equipment Shop" blip on your map
3. **Interact**: Use ox_target (third eye) on the shop ped
4. **Browse**: Select "Browse Illegal Equipment" to open the shop menu
5. **Purchase**: Choose items, select quantity, and purchase with cash/bank/black money

### Shop Configuration

Edit `config.lua` to customize shops:

```lua
Config.Shop = {
    Enabled = true,
    Locations = {
        {
            coords = vector4(x, y, z, heading),
            pedModel = `model_hash`,
            pedHeading = heading,
            pedScenario = 'scenario_name',
            blip = {enabled = true, sprite = 52, color = 1, scale = 0.8, label = 'Illegal Equipment Shop'}
        },
    },
    Items = {
        {item = 'card_skimmer', label = 'Card Skimmer', price = 5000, stock = -1},
        -- Add more items
    },
}
```

## Usage

### Skimming Credit Cards

1. Equip a **card_skimmer** in your inventory
2. Approach a player or NPC (within 2 meters)
3. Use ox_target to select "Skim Credit Card"
4. Wait for the progress bar to complete
5. If successful, you'll receive a **credit_card** item with a random quality tier

### Card Quality System

Cards are randomly assigned one of five quality tiers when skimmed:
- **Bronze** (40% chance): $500-$1,500 value, 90% success rate
- **Silver** (30% chance): $1,500-$3,000 value, 80% success rate
- **Gold** (20% chance): $3,000-$5,000 value, 70% success rate
- **Platinum** (8% chance): $5,000-$8,000 value, 60% success rate
- **Black** (2% chance): $8,000-$15,000 value, 50% success rate

Higher quality cards offer:
- Higher potential rewards
- Lower success rates (more risk)
- Higher police alert chances

### Item Decay System

Cards have expiration times using ox_inventory's decay system:
- **Credit Cards**: Expire after 48 hours (2 days)
- Equipment items (skimmer, reader) do not decay

Expired cards cannot be used at ATMs. ox_inventory will automatically show decay timers and warn players when items are about to expire.

### Injecting Cards into ATMs

1. Have a **credit_card** and **card_reader** in your inventory
2. Approach any ATM (within 2 meters)
3. Use ox_target to select "Inject Credit Card"
4. Wait for the progress bar to complete
5. If successful, you'll receive money based on:
   - **Card Quality**: Higher tier cards = higher rewards
   - **Success Rate**: Each tier has different success chances
   - **Banking System**: Money goes to bank (okok/qb) or black_money item
6. Failed injections return the card to your inventory

## Dispatch Integration

The script automatically detects which dispatch system you're using. You can also manually set it in `config.lua`:

```lua
Config.Dispatch.System = 'rcore' -- or 'lb', 'wasabi', 'cd', 'auto', 'none'
```

When set to `'auto'`, the script will detect which dispatch system is running.

## Commands

- `/ccskim_give [id] [item] [amount]` - Admin command to give items (for testing)
  - Items: `skimmer`, `card`, `reader`
- `/ccskim_limits [id]` - Check skimming limits for a player (Admin)
- `/ccskim_reset [id]` - Reset skimming limits for a player or all players (Admin)

## Security Features

- Server-side validation for all actions
- Cooldown system (both client and server-side with caching)
- Item requirement checks
- Distance validation
- Success chance system
- Local caching for improved performance and reduced load

## Performance Features

- **Local Caching**: 
  - Player data caching (30s expiration)
  - Item count caching (60s expiration)
  - Inventory caching (10s expiration)
  - Cooldown caching
  - NPC caching (2s expiration)
  - ATM caching (1s expiration)
  - Automatic cache cleanup
  - Cache invalidation on item changes

- **Optimizations**:
  - Squared distance calculations (faster than sqrt)
  - Hash table lookups for NPC models
  - Cached target model checks
  - Optimized inventory searches
  - Efficient loop iterations
  - Reduced redundant calculations

## Global Limits System

The script includes a comprehensive limit system to prevent 24/7 skimming:

- **Hourly Limit**: Maximum cards that can be skimmed per hour (default: 10)
- **Daily Limit**: Maximum cards that can be skimmed per day (default: 50)
- **Automatic Reset**: Hourly limits reset every hour, daily limits reset at midnight
- **Player Tracking**: Each player has individual limits tracked by citizenid
- **Admin Commands**: Admins can check and reset limits for players

Configure limits in `config.lua`:
```lua
Config.Skimming = {
    MaxPerHour = 10,  -- Cards per hour
    MaxPerDay = 50,   -- Cards per day
    ResetTime = 3600, -- Hourly reset time (seconds)
    DailyResetHour = 0, -- Daily reset hour (0-23)
}
```

## Police Alert System

The script now includes enhanced police alerting:

- **Always Alert**: Option to always alert police on every skim attempt
- **Success Alerts**: Alert police when skimming succeeds
- **Failure Alerts**: Alert police when skimming fails
- **Configurable**: Fully configurable in config.lua
- **Multiple Systems**: Works with all supported dispatch systems

Configure alerts in `config.lua`:
```lua
Config.Dispatch = {
    AlwaysAlertOnSkim = true,  -- Always alert on skim attempt
    AlertOnSuccess = true,     -- Alert on successful skim
    AlertOnFailure = true,      -- Alert on failed skim
}
```

## Support

For issues or questions, please check:
- Your server console for errors
- Ensure all dependencies are installed
- Verify items are added to your items.lua
- Check that ox_lib, ox_target, and ox_inventory are properly installed

Or open a ticket in our discord server https://discord.gg/2Csrm5MaRd

## License

This script is provided as-is. Modify and use as needed for your server.

