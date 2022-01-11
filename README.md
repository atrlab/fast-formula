select  pf.RESULT_NAME
        , pf.RESULT_RULE_TYPE
        , pr.formula_id
        , ff.formula_name
        , ffb.base_formula_name
        , petf.base_element_name target_element
        , pet.reporting_name
        , pet.element_name
        , pivf.base_name input_name 
        ,(select base_element_name from pay_element_types_f where element_type_id = pr.element_type_id) main_element
        ,(select element_name from PAY_ELEMENT_TYPES_TL where element_type_id = pr.element_type_id and language = 'US') main_element_name
from       PAY_FORMULA_RESULT_RULES_F pf
            ,pay_element_types_f petf 
            ,pay_input_values_f  pivf
            ,PAY_STATUS_PROC_RULES_F pr
            ,FF_FORMULAS_TL ff
            ,FF_FORMULAS_B_F ffb
            ,PAY_ELEMENT_TYPES_TL pet
where   pf.input_value_id = pivf.input_value_id
and     petf.element_type_id = pf.element_type_id
and     pet.element_type_id = pf.element_type_id
and     pet.element_name in ( 'Medicare Employee Tax', 'Medicare Employer Tax', 'Social Security Employee Tax', 'Social Security Employer Tax')
and     pivf.base_name = 'Tax Calculated'
and     pr.STATUS_PROCESSING_RULE_ID = pf.STATUS_PROCESSING_RULE_ID
and     pr.formula_id = ff.formula_id
and     ffb.formula_id = ff.formula_id
and     ff.language = 'US'
