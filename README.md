# IntrinsicValueSite
Pulls data using yfinance, Calculates discount rate from intrinsic value using a cashflow, eps, and blended models. also graphs the change in the models' calculation from before and after most recent earnings date. python backend with flask, html frontend.
may add other models later but the initial calculates value per share as follows: (r1 and r2 are short and long term cashflow growth)
victor_valuation(ttm_cashflow, r1, r2, discount_rate, total_debt, shares_outstanding, eps, pe_ratio, cash, EPSG):
    
    projected_cashflow = [ttm_cashflow] + [ttm_cashflow * (1 + r1)**year for year in range(1, 4)]
    terminal_value = projected_cashflow[-1] * (1 + r2) / (discount_rate - r2)
    projected_cashflow += [terminal_value]

    npv_cashflows = sum([cf / (1 + discount_rate)**(i+1) for i, cf in enumerate(projected_cashflow)])
    eps_iv = eps * (1 + EPSG) * pe_ratio
    equity_value = npv_cashflows + cash - total_debt
    value_per_share_fcf = equity_value / shares_outstanding
    value_per_share_eps = eps_iv
    combined_value_per_share = (2/3) * value_per_share_fcf + (1/3) * value_per_share_eps
